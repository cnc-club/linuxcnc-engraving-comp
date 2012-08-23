CNC-CLUB.ru (.com) Presents
===========================

Component to scan the surface and add compensation
to Z axis to eliminate surfase inflattness.  

Find details at http://www.cnc-club.ru 
(http://cnc-club.ru/forum/viewtopic.php?f=15&t=521&start=220&p=19525#p19525)


Install
===================

Save files to your ini directory. 
Change custom_postgui.hal signal names if needed. 

Add these lines to ini file: 

Into [HAL] section:
POSTGUI_HALFILE = custom_postgui.hal

Into [DISPLAY] section:
PYVCP = comrensation_pyvcp.xml

Operate
===================

1. Scan the surface using Gcode like this:
<pre>
M64 P0  (turn off compensation)
T1 M6 (Install probe)
#&lt;h&gt; = 90 
#&lt;w&gt; = 90 
#&lt;x0&gt; = 110
#&lt;y0&gt; = 90
#&lt;step&gt; = 20
#&lt;search_feed&gt; = 100
#&lt;latch_feed&gt; = 1
#&lt;safe_z&gt; = 3
#&lt;search_z&gt; = -3

(PROBEOPEN filename.txt)

G0 #&lt;safe_z&gt;
#&lt;y&gt; = #&lt;y0&gt;
o101 while [#&lt;y&gt; LT #&lt;h&gt;+#&lt;y0&gt;]
  #&lt;x&gt; = #&lt;x0&gt;
  o102 while [#&lt;x&gt; LT #&lt;w&gt;+#&lt;x0&gt;]
    G0 X#&lt;x&gt; Y#&lt;y&gt; 
    F[#&lt;search_feed&gt;]
    G38.3 [Z#&lt;search_z&gt;]
    F[#&lt;latch_feed&gt;]
    G38.5 [Z#&lt;safe_z&gt;]
   G0 #&lt;safe_z&gt; 
    #&lt;x&gt; = [#&lt;x&gt;+#&lt;step&gt;]
  o102 endwhile
  #&lt;y&gt; = [#&lt;y&gt;+#&lt;step&gt;]
o101 endwhile

(PROBECLOSE)
</pre>

2. Reset component's height map by pressing Reset button at pyvcp panel or throught Gcode
M65 P1 (Reset compenstation)
G4 P1
M64 P1 (Reset compenstation)
G4 P4 (Wait for 4 seconds, just in case)

3. Turn on the compensation by checking Enable check box and prepare to see the magick :)!
Or:
M64 P0  (Turn on the compensation)
