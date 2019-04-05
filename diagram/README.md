# General diagram
<p><img src="../images/uaf_infrastructure.jpg" alt="Infrastructure diagram"></p>
**a**. The Frontend queries the schedds for idle jobs<br>
**b**. The schedds advertise its jobs' classAds to the Collector<br>
**c**. The Frontned request pilots to the factory<br>
**d**. The Factory sends pilots to the Sites' Gatekeepers<br>
**e**. The pilot gets queued into the Site's local batch system<br>
**f**. Similar to (b) but it happens inside the Site's local batch(in case it is condor)<br>
**g**. The Startds advertise themselves to the Collector<br>
**h**. Inside a Site Startd a pilot starts and in turn executes a pilot Startd that will advertise itself not to the Site's Collector as in (g) but the global one. <br>
