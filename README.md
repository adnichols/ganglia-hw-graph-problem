ganglia-hw-graph-problem
========================
All,
  I'm trying to apply Holt-Winters to a few of our rrd's and have run into a problem which seems specific to the \_\_SummaryInfo\_\_ rrds. When I apply the Holt-Winters algorithm to both the host rrds as well as the \_\_SummaryInfo\_\_ rrds, the host rrds work fine however the \_\_SummaryInfo\_\_ do not. This note will focus on the \_\_SummaryInfo\_\_ rrds.

These metrics are stored in an rrd of type COUNTER. I have also observed this behavior on the same metric set to type DERIVE. 

I am using the rrd_hwreapply.pl script (http://rrfw.sourceforge.net/rrdman/rrd_hwreapply.pod.html) to apply holt-winters to the rrds and for simplicity right now I'm only using the following command line:

$ sudo -u nobody cp cron.webServiceRequestCounter.Counter.rrd cron.webServiceRequestCounter.Counter.rrd.bak && sudo -u nobody rrd_hwreapply cron.webServiceRequestCounter.Counter.rrd.bak cron.webServiceRequestCounter.Counter.rrd --defaults --force

As soon as I run this command, the primary metric (cron.webServiceRequestCounter.Counter) will stop rendering new values on the graph. It appears, however, that updates are still being stored in the rrd so I suspect a rendering problems - perhaps with rrdtool, but I'm not sure. 

Here's an example of the graph leading up to applying the HW configurations:

Before applying change

![image](https://github.com/adnichols/ganglia-hw-graph-problem/blob/master/pre-hw-graph.png?raw=true)

After applying the change:

![image](https://github.com/adnichols/ganglia-hw-graph-problem/blob/master/post-hw-graph.png?raw=true)

You can see that after the HW change is applied, data is still stored in the rrd however it is stored differently. 

I applied the HW changes around 09:15 - see the shift here:

                        <!-- 2012-10-22 09:12:30 MDT / 1350918750 --> <row><v>0.0000000000e+00</v><v>1.7573333333e+02</v></row>
                        <!-- 2012-10-22 09:12:45 MDT / 1350918765 --> <row><v>0.0000000000e+00</v><v>1.5520000000e+02</v></row>
                        <!-- 2012-10-22 09:13:00 MDT / 1350918780 --> <row><v>0.0000000000e+00</v><v>1.5986666667e+02</v></row>
                        <!-- 2012-10-22 09:13:15 MDT / 1350918795 --> <row><v>0.0000000000e+00</v><v>1.7153333333e+02</v></row>
                        <!-- 2012-10-22 09:13:30 MDT / 1350918810 --> <row><v>0.0000000000e+00</v><v>1.5520000000e+02</v></row>
                        <!-- 2012-10-22 09:13:45 MDT / 1350918825 --> <row><v>0.0000000000e+00</v><v>1.3366666667e+02</v></row>
                        <!-- 2012-10-22 09:14:00 MDT / 1350918840 --> <row><v>0.0000000000e+00</v><v>1.3920000000e+02</v></row>
                        <!-- 2012-10-22 09:14:15 MDT / 1350918855 --> <row><v>0.0000000000e+00</v><v>1.6160000000e+02</v></row>
                        <!-- 2012-10-22 09:14:30 MDT / 1350918870 --> <row><v>0.0000000000e+00</v><v>1.6600000000e+02</v></row>
                        <!-- 2012-10-22 09:14:45 MDT / 1350918885 --> <row><v>0.0000000000e+00</v><v>1.5040000000e+02</v></row>
                        <!-- 2012-10-22 09:15:00 MDT / 1350918900 --> <row><v>0.0000000000e+00</v><v>1.3873333333e+02</v></row>
                        <!-- 2012-10-22 09:15:15 MDT / 1350918915 --> <row><v>0.0000000000e+00</v><v>1.5333333333e+02</v></row>
                        <!-- 2012-10-22 09:15:30 MDT / 1350918930 --> <row><v>7.0035125000e+05</v><v>1.7825672150e+08</v></row>
                        <!-- 2012-10-22 09:15:45 MDT / 1350918945 --> <row><v>4.2027495000e+05</v><v>1.0695403290e+08</v></row>
                        <!-- 2012-10-22 09:16:00 MDT / 1350918960 --> <row><v>1.6012083333e+02</v><v>0.0000000000e+00</v></row>
                        <!-- 2012-10-22 09:16:15 MDT / 1350918975 --> <row><v>1.6163750000e+02</v><v>0.0000000000e+00</v></row>
                        <!-- 2012-10-22 09:16:30 MDT / 1350918990 --> <row><v>1.6239880952e+02</v><v>0.0000000000e+00</v></row>
                        <!-- 2012-10-22 09:16:45 MDT / 1350919005 --> <row><v>1.5191785714e+02</v><v>0.0000000000e+00</v></row>
                        <!-- 2012-10-22 09:17:00 MDT / 1350919020 --> <row><v>1.4803611111e+02</v><v>0.0000000000e+00</v></row>
                        <!-- 2012-10-22 09:17:15 MDT / 1350919035 --> <row><v>1.6262222222e+02</v><v>0.0000000000e+00</v></row>
                        <!-- 2012-10-22 09:17:30 MDT / 1350919050 --> <row><v>1.6964761905e+02</v><v>0.0000000000e+00</v></row>
                        <!-- 2012-10-22 09:17:45 MDT / 1350919065 --> <row><v>1.6213904762e+02</v><v>0.0000000000e+00</v></row>
                        <!-- 2012-10-22 09:18:00 MDT / 1350919080 --> <row><v>1.5585333333e+02</v><v>0.0000000000e+00</v></row>
                        <!-- 2012-10-22 09:18:15 MDT / 1350919095 --> <row><v>1.5385333333e+02</v><v>0.0000000000e+00</v></row>
                        <!-- 2012-10-22 09:18:30 MDT / 1350919110 --> <row><v>1.4950666667e+02</v><v>0.0000000000e+00</v></row>
                        <!-- 2012-10-22 09:18:45 MDT / 1350919125 --> <row><v>1.5220000000e+02</v><v>0.0000000000e+00</v></row>
                        <!-- 2012-10-22 09:19:00 MDT / 1350919140 --> <row><v>1.5200000000e+02</v><v>0.0000000000e+00</v></row>
                        <!-- 2012-10-22 09:19:15 MDT / 1350919155 --> <row><v>1.5960000000e+02</v><v>0.0000000000e+00</v></row>
                        <!-- 2012-10-22 09:19:30 MDT / 1350919170 --> <row><v>1.5511904762e+02</v><v>0.0000000000e+00</v></row>
                        <!-- 2012-10-22 09:19:45 MDT / 1350919185 --> <row><v>1.5466428571e+02</v><v>0.0000000000e+00</v></row>
                        <!-- 2012-10-22 09:20:00 MDT / 1350919200 --> <row><v>1.4741666667e+02</v><v>0.0000000000e+00</v></row>
                        <!-- 2012-10-22 09:20:15 MDT / 1350919215 --> <row><v>1.6173333333e+02</v><v>0.0000000000e+00</v></row>
                        <!-- 2012-10-22 09:20:30 MDT / 1350919230 --> <row><v>1.6886666667e+02</v><v>0.0000000000e+00</v></row>
                        <!-- 2012-10-22 09:20:45 MDT / 1350919245 --> <row><v>1.5666666667e+02</v><v>0.0000000000e+00</v></row>
                        <!-- 2012-10-22 09:21:00 MDT / 1350919260 --> <row><v>1.4283750000e+02</v><v>0.0000000000e+00</v></row>
                        <!-- 2012-10-22 09:21:15 MDT / 1350919275 --> <row><v>1.3890027778e+02</v><v>0.0000000000e+00</v></row>

Note how at 2012-10-22 09:16:00 values start getting logged to the 1st column instead of the 2nd column. 

I'm not sure if this is a problem with rrdtool or with Ganglia - here are the versions of things I'm running:

rrdtool:
rrdtool-1.4.5-1.wrl.x86_64
rrdtool-perl-1.4.5-1.wrl.x86_64

libganglia-3.4.0-1.x86_64
ganglia-gmond-3.4.0-1.x86_64
ganglia-gmetad-3.4.0-1.x86_64
ganglia-gmond-modules-python-3.4.0-1.x86_64

Ganglia RPM's were built with rrd 1.4.5. 