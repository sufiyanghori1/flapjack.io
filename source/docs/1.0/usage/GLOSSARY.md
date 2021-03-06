## Glossary

As implemented for the reporting API functions, these are placeholder terms only and open for discussion.

<dl>
<dt>Entity</dt>
<dd>A system or group of systems being monitored. An entity can be as simple as a VM or as complex as a data center containing racks of servers running farms of VMs.</dd>
<dt>Check</dt>
<dd>A particular attribute of an entity which for which we care about changes in state</dd>
<dt>Outage</dt>
<dd>Period of time from when a check goes into the `CRITICAL` state to when it comes out of that state again.</dd>
<dt>Scheduled maintenance</dt>
<dd>Periods of time explicitly created by external actors to denote that maintenance is scheduled to occur at these times.</dd>
<dt>Unscheduled maintenance</dt>
<dd>Periods of time explicitly created by external actors to denote that maintenance is happening or has happened at these times.</dd>
<dt>Downtime</dt>
<dd>Outages minus scheduled maintenances across any given time period.</dd>
</dl>
