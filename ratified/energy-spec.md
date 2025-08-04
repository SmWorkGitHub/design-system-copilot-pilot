---
seo:
  title: Energy and Power Reporting for Sustainability v0.85 | HPE GreenLake Platform
toc:
  enable: true
---

# Energy and Power Reporting for Sustainability Spec v0.85

<!DOCTYPE html>
<!-- I've implemented this spec in HTML rather than Markdown because
Redocly presented text at outline level 3 as code samples.
As Markdown accepts HTML, this should be ok. - Steve Anderson -->
<html>
<head>
<meta name="generator" content=
"HTML Tidy for HTML5 for Apple macOS version 5.8.0">
<title>Energy and Power Reporting for Sustainability v0.85</title>
</head>
<body>
<ol type="A">
<li>
<p>For <strong>inclusion in sustainability reporting on the HPE
GreenLake Cloud Platform</strong>, mains-connected
devices<sup>1</sup> MUST make available energy or power values that
adhere to the following criteria:</p>
<ol type="1">
<li>
<p>Device MUST provide at least one of (in order of descending
preference):</p>
<ol type="a">
<li>
<p>Energy<sup>2</sup>, which must include</p>
<ol type="i">
<li>
<p>Accumulated energy consumption since a specified epoch (e.g.
standard defined epochs, or alternately manufacture, first install
/ deploy, last power cycle)</p>
</li>
<li>
<p>Start time or definition of the specified epoch</p>
</li>
<li>
<p>Time of energy measurement</p>
</li>
</ol>
</li>
<li>
<p>Average power<sup>2</sup>, which must include</p>
<ol type="i">
<li>
<p>Average input power (before any AC or DC conversions i.e. from
the wall or AC inlet)</p>
</li>
<li>
<p>Averaging interval length or averaging interval end time</p>
</li>
<li>
<p>Averaging interval start time</p>
</li>
</ol>
</li>
<li>
<p>Instantaneous power<sup>2</sup>, which must include</p>
<ol type="i">
<li>
<p>Instantaneous power (before any AC or DC conversions, i.e. from
the wall or AC inlet)</p>
</li>
<li>
<p>Time of instantaneous power measurement</p>
</li>
</ol>
</li>
</ol>
</li>
<li>
<p>Provided values SHOULD have an accuracy of at least:</p>
<ol type="a">
<li>
<p>+/- 10% if device maximum power rating is less than or equal to 60 W</p>
</li>
<li>
<p>+/- 5% if device maximum power rating is greater than 60 W</p>
</li>
</ol>
</li>
<li>
<p>Provided values SHOULD be measured by the power
supply<sup>2</sup>. If appropriate outputs are not available, then
energy or average power MUST be calculated according to the
following criteria:</p>
<ol type="a">
<li>
<p>Energy equal to sum of instantaneous power measurements times
measurement time interval</p>
</li>
<li>
<p>Average power values equal to the arithmetic mean of
instantaneous power measurements</p>
</li>
<li>
<p>Instantaneous power sampling frequency sufficient to achieve the
desired accuracy in item 2</p>
</li>
</ol>
</li>
<li>
<p>Provided values MUST be updated and provided at least:</p>
<ol type="a">
<li>
<p>Once every 1 hour for energy and average power</p>
</li>
<li>
<p>Once every 30 seconds for instantaneous power</p>
</li>
<li>
<p>OR less frequently if it can be shown that energy or power do
not vary significantly<sup>3</sup> over the updating time scale AND
the lower frequency still fulfills the accuracy requirements in
item 2</p>
</li>
</ol>
</li>
<li>
<p>Energy or power values SHOULD be provided per power supply or
power cord</p>
</li>
<li>
<p>Accuracy or uncertainty of energy or power values SHOULD be
provided</p>
</li>
<li>
<p>During periods of time during which measurements cannot be made,
energy or power SHOULD be estimated if feasible or reported as 0 if
estimation is not accurate or feasible</p>
</li>
</ol>
</li>
<li>
<p>All mains-connected devices <strong>managed through the HPE
GreenLake Cloud Platform</strong> with a power rating greater than
10 W SHOULD make available energy or power values according to the
criteria in paragraph A.</p>
</li>
<li>
<p>Non-mains-connected entities<sup>4</sup> with significant or
highly variable consumption <strong>associated with the HPE
GreenLake Cloud Platform</strong> SHOULD make available energy or
power values. If energy or power values are made available, they
MUST adhere to the criteria in paragraph A and additionally MUST
report their power source.</p>
</li>
<li>
<p>Energy or power values provided by devices and entities are made
available to the HPE GreenLake Cloud Platform according to the
following criteria:</p>
<ol type="1">
<li>
<p>Data SHOULD be transferred at least once per week and no more
than once per hour</p>
</li>
<li>
<p>Data SHOULD be retained outside of the HPE GreenLake Cloud
Platform for at least one week</p>
</li>
<li>
<p>All data outlined in paragraph A clauses 1, 2, and 3 above MUST
be made available</p>
</li>
<li>
<p>The type of value being reported (energy, average power, or
instantaneous power) MUST be made available</p>
</li>
</ol>
</li>
</ol>
<p><sup>1</sup> <em>Mains-connected device</em> = any device
drawing from the mains power and <strong>not</strong> drawing power
from another managed device</p>
<p><sup>2</sup> Different power supplies may provide either energy,
average power, or instantaneous power as standard outputs.
Technologists must determine which quantity their power supply
outputs and whether that output meets the required specifications.
If not, it may be necessary to process raw data in firmware or
software before providing it to GLCP.</p>
<p>Energy and power are related in a similar way to how distance
and speed are related. Power is the rate at which a device consumes
energy, similar to how speed is the rate at which an object covers
distance. You can think of energy like the odometer on your car and
power like the speedometer.</p>
<p>Most IT devices record periodic measurements of the power
consumed by the device or by subsystems within the device, such as
the CPUs, memory, or fans. The energy consumed by an entity during
a specified time period is the power drawn by that entity during
the time period multiplied by the length of the time period. The
energy for a longer time period can be found by adding together the
energy over shorter time periods.</p>
<p>For example, consider a server that measures and reports the
power it consumes in Watts (W) every minute. Let’s say the server
reported drawing 235W at 10:43 and 300 W at 10:44. In the minute
between 10:43 and 10:44, the energy consumed by the server was
<span class=
"math inline">235&nbsp;<em>W</em>&nbsp; × 1min  = 235&nbsp;<em>W</em><em>m</em><em>i</em><em>n</em></span>.
Or, to express the answer in the more common units of kWh, the
energy consumed was <span class=
"math inline">0.235&nbsp;<em>k</em><em>W</em>&nbsp; × 0.0167&nbsp;<em>h</em> = 0.00392&nbsp;<em>k</em><em>W</em><em>h</em></span>
because 235 W = 0.235 kW and 1 min = 0.0167 h. To find the energy
consumed during the two minutes between 10:43 and 10:45, we simply
repeat the energy calculation with the power from the second minute
<span class=
"math inline">0.300&nbsp;<em>k</em><em>W</em>&nbsp; × 0.0167&nbsp;<em>h</em> = 0.005&nbsp;<em>k</em><em>W</em><em>h</em></span>
and add that energy to the energy from the first minute to get
<span class=
"math inline">0.00392&nbsp;<em>k</em><em>W</em><em>h</em> + 0.005&nbsp;<em>k</em><em>W</em><em>h</em> = 0.00892&nbsp;<em>k</em><em>W</em><em>h</em></span>
consumed between 10:43 and 10:45.</p>
<p>Care must be taken to understand whether an entity is reporting
average power or instantaneous power. Most <em>measurements</em> of
electrical power are effectively instantaneous (although the
electronics for making the measurement may have some timescale
associated with them, this timescale is typically much shorter than
the timescale of any changes in power draw from the entity).
However, the systems implemented to report that power (e.g.
firmware) may average many of these instantaneous power
measurements and either still report the resulting quantity as
instantaneous power or fail to specify whether the result is
instantaneous or average power.</p>
<p><sup>3</sup> Customers want to be able to correlate changes made
in use of devices with changes in energy consumption and emissions,
so updating of values needs to be frequent enough to support this
correlation.</p>
<p><sup>4</sup> <em>Non-mains-connected entity</em> = any powered
device or sub-component of a device that is powered through another
device and <strong>not</strong> drawing directly from mains power,
e.g PoE-powered devices, subsystems within a server like CPU or
memory</p>
<p><b>Original publication date (yyyy-mm-dd):</b> 2023-11-14</p>
</body>
</html>
