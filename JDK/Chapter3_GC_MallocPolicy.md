JVM GC�㷨 CMS ���(ת)

ǰ��
CMS��ȫ��Concurrent Low Pause Collector����jdk1.4���ڰ汾��ʼ�������gc�㷨����jdk5��jdk6�еõ��˽�һ���Ľ���������Ҫ�ʺϳ����Ƕ���Ӧʱ�����Ҫ������ ���ڶ���������Ҫ���ܹ��������������̺߳�Ӧ���̹߳���������Դ������Ӧ���д��ڱȽ϶�ĳ��������ڵĶ����Ӧ�á�CMS�����ڶ�tenured generation�Ļ��գ�Ҳ�������ϴ��Ļ��գ�Ŀ���Ǿ�������Ӧ�õ���ͣʱ�䣬����full gc�����ļ��ʣ����ú�Ӧ�ó����̲߳��������������߳������������ϴ��������ǵ�Ӧ���У���Ϊ�л���Ĵ��ڣ����Ҷ�����Ӧʱ��Ҳ�бȽϸߵ�Ҫ�����ϣ ���ܳ���ʹ��CMS�����Ĭ�ϵ�server��JVMʹ�õĲ����ռ������Ա��ø��̵��������յ���ͣʱ�䣬��߳������Ӧ�ԡ�

 

CMS�ռ�����

CMS����û����ͣ�����������ζ���ͣ��������б�������㷨�ĳ���ͣ�������ռ�������������
��ʼ���(CMS-initial-mark) -> �������(CMS-concurrent-mark) -> ���±��(CMS-remark) -> �������(CMS-concurrent-sweep) ->��������״̬�ȴ��´�CMS�Ĵ���(CMS-concurrent-reset)��
���е�1��3����������Ҫ��ͣ���е�Ӧ�ó����̵߳ġ���һ����ͣ��root����ʼ��Ǵ��Ķ�������׶γ�Ϊ��ʼ��ǣ��ڶ�����ͣ���ڲ������֮�� ��ͣ����Ӧ�ó����̣߳����±�ǲ�����ǽ׶���©�Ķ����ڲ�����ǽ׶ν��������״̬�ĸ��µ��£�����һ����ͣ��Ƚ϶̣��ڶ�����ͣͨ����Ƚϳ������� remark����׶ο��Բ��б�ǡ�

��������ǡ������������������׶ε���ν��������ָһ�����߶�����������̺߳�Ӧ�ó����̲߳��������У����������̲߳�����ͣӦ�ó����ִ�У�������ж���һ������������ô�����ռ��߳̽���Ӧ���߳��ڲ�ͬ�Ĵ����������У���Ȼ�������Ŀ������ǻή��Ӧ�õ���������Remark�׶εĲ��У���ָ��ͣ������Ӧ�ó��������һ����Ŀ���������ս��̽��в��б�ǣ���ʱ��Ӧ���߳�����ͣ�ġ�

CMS��young generation�Ļ��ղ��õ���Ȼ�ǲ��и����ռ����������Paralle gc�㷨��һ�µġ�

 

��������
1������CMS��-XX:+UseConcMarkSweepGC��
2��CMSĬ�������Ļ����߳���Ŀ�� (ParallelGCThreads + 3)/4) ���������Ҫ��ȷ�趨������ͨ��-XX:ParallelCMSThreads=20���趨,����ParallelGCThreads��������Ĳ����ռ��߳���
3��CMS�ǲ����������Ƭ�ģ����Ϊ�˷�ֹ����Ƭ����full gc��ͨ���Ὺ��CMS�׶ν��кϲ���Ƭѡ�-XX:+UseCMSCompactAtFullCollection���������ѡ��һ���̶��ϻ�Ӱ�����ܣ�������blog��˵Ҳ�����ͨ�������ʵ���CMSFullGCsBeforeCompaction���������ܣ�δʵ����
4.Ϊ�˼��ٵڶ�����ͣ��ʱ�䣬��������remark: -XX:+CMSParallelRemarkEnabled�����remark���ǹ����Ļ������Կ���-XX:+CMSScavengeBeforeRemarkѡ�ǿ��remark֮ǰ��ʼһ��minor gc������remark����ͣʱ�䣬������remark֮��Ҳ��������ʼ��һ��minor gc��

5.Ϊ�˱���Perm���������full gc�����鿪��CMS����Perm��ѡ�
+CMSPermGenSweepingEnabled -XX:+CMSClassUnloadingEnabled

6.Ĭ��CMS����tenured generationմ��68%��ʱ��ʼ����CMS�ռ������������ϴ�����������ô�죬����ϣ������CMS�����Ļ��������ʵ����ߴ�ֵ��
-XX:CMSInitiatingOccupancyFraction=80

�����޸ĳ�80%մ����ʱ��ſ�ʼCMS���ա�

7.������Ĳ����ռ��߳���Ĭ����(cpu <= 8) ? cpu : 3 + ((cpu * 5) / 8)�������ϣ����������߳���������ͨ��-XX:ParallelGCThreads= N ��������

8.�����ص㣬�ڳ���������һЩ���������磺

-server -Xms1536m -Xmx1536m -XX:NewSize=256m -XX:MaxNewSize=256m -XX:PermSize=64m 
-XX:MaxPermSize=64m -XX:-UseConcMarkSweepGC -XX:+UseCMSCompactAtFullCollection 
-XX:CMSInitiatingOccupancyFraction=80 -XX:+CMSParallelRemarkEnabled 
-XX:SoftRefLRUPolicyMSPerMB=0 

��Ҫ��������������ѹ�⻷���в�����Щ������ϵͳ�ı��֣���ʱ����Ҫ��GC��־�鿴�������Ϣ����˼��ϲ�����
-verbose:gc -XX:+PrintGCTimeStamps -XX:+PrintGCDetails -Xloggc:/home/test/logs/gc.log

�������൱��һ��ʱ���ڲ鿴CMS�ı��������CMS����־�������������

���ƴ���
4391.322: [GC [1 CMS-initial-mark: 655374K(1310720K)] 662197K(1546688K), 0.0303050 secs] [Times: user=0.02 sys=0.02, real=0.03 secs] 
4391.352: [CMS-concurrent-mark-start] 
4391.779: [CMS-concurrent-mark: 0.427/0.427 secs] [Times: user=1.24 sys=0.31, real=0.42 secs] 
4391.779: [CMS-concurrent-preclean-start] 
4391.821: [CMS-concurrent-preclean: 0.040/0.042 secs] [Times: user=0.13 sys=0.03, real=0.05 secs] 
4391.821: [CMS-concurrent-abortable-preclean-start] 
4392.511: [CMS-concurrent-abortable-preclean: 0.349/0.690 secs] [Times: user=2.02 sys=0.51, real=0.69 secs] 
4392.516: [GC[YG occupancy: 111001 K (235968 K)]4392.516: [Rescan (parallel) , 0.0309960 secs]4392.547: [weak refs processing, 0.0417710 secs] [1 CMS-remark: 655734K(1310720K)] 766736K(1546688K), 0.0932010 secs] [Times: user=0.17 sys=0.00, real=0.09 secs] 
4392.609: [CMS-concurrent-sweep-start] 
4394.310: [CMS-concurrent-sweep: 1.595/1.701 secs] [Times: user=4.78 sys=1.05, real=1.70 secs] 
4394.310: [CMS-concurrent-reset-start] 
4394.364: [CMS-concurrent-reset: 0.054/0.054 secs] [Times: user=0.14 sys=0.06, real=0.06 secs] 
���ƴ���
 

���п��Կ���CMS-initial-mark�׶���ͣ��0.0303050�룬��CMS-remark�׶���ͣ��0.0932010�룬���������ͣ���ܹ�ʱ����0.123506�룬Ҳ����123�������ҡ����ζ���ͣ��ʱ��֮����200���¿��Գ�Ϊ��������

������ܿ�����������fail����full gc��Prommotion failed��Concurrent mode failed��

Prommotion failed����־��������������

[ParNew (promotion failed): 320138K->320138K(353920K), 0.2365970 secs]42576.951: [CMS: 1139969K->1120688K( 
166784K), 9.2214860 secs] 1458785K->1120688K(2520704K), 9.4584090 secs] 

�������Ĳ��������ھ����ռ䲻�����Ӷ������ϴ�ת�ƶ������ϴ�û���㹻�Ŀռ���������Щ���󣬵���һ��full gc�Ĳ���������������İ취��������ȫ�෴��������������ռ䡢�������ϴ�����ȥ�������ռ䡣 ��������ռ���ǵ���-XX:SurvivorRatio���������������Eden����Survivor���Ĵ�С��ֵ��Ĭ����32��Ҳ����˵Eden���� Survivor����32����С��Ҫע��Survivo�����������ģ����Surivivor��ʵռ����young genertation��1/34����С�������������survivor�����ö�������survitor������һ�㣬���ٽ������ϴ��Ķ���ȥ�������� ����뷨���ô󲿷ֲ������ϻ��յ����ݾ���������ϴ����ӿ����ϴ��Ļ���Ƶ�ʣ��������ϴ����ǵĿ����ԣ������ͨ����-XX:SurvivorRatio ���óɱȽϴ��ֵ������65536)�������������ǵ�Ӧ���У���young generation���ó�256M�����ֵ�����˵�Ƚϴ��ˣ��������ռ����ó�Ĭ�ϴ�С(1/34)����ѹ�����������û�г���prommotion failed������������Ƚϴ󣬴�GC��־������minor gc��ʱ��Ҳ��5-20�����ڣ������Խ��ܣ�����ݲ�������

Concurrent mode failed�Ĳ���������CMS�������ϴ����ٶ�̫�����������ϴ���CMS���ǰ�ͱ�մ��������full gc�������������Ĳ������ǵ�С-XX:CMSInitiatingOccupancyFraction������ֵ����CMS�����Ƶ���Ĵ������������ϴ���մ���Ŀ��ܡ����ǵ�Ӧ����ʱ���رȽϵͣ����������������ϴ��������ǳ������������ʱ���ô˲���Ϊ80����ѹ�⻷���£���������ı��ֻ����ԣ�û�г��ֹ�Concurrent mode failed��