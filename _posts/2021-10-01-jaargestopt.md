---
layout: post
title: Een jaar alweer
date: 2021-10-01 15:30:00 +0100
categories: Health
---

![roken]({{ site.baseurl }}/assets/hotsmoke.png)  

### Niet bijzonder, wel trots

Het is vandaag precies een jaar gelden dat ik samen met Daniël mijn laatste sigaret rookte. Terugkijkend was het vooral een hele vreemde gewoonte.

<script>
var montharray=new Array("Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec")
function countup(yr,m,d){
var today=new Date()
var todayy=today.getYear()
if (todayy < 1000)
todayy+=1900
var todaym=today.getMonth()
var todayd=today.getDate()
var todaystring=montharray[todaym]+" "+todayd+", "+todayy
var paststring=montharray[m-1]+" "+d+", "+yr
var difference=(Math.round((Date.parse(todaystring)-Date.parse(paststring))/(24*60*60*1000))*1)
difference+=" dagen"
document.write("We blijven doortellen. We zitten nu op "+difference+".")
}
//enter the count up date using the format year/month/day
countup(2020,10,01)
</script>
