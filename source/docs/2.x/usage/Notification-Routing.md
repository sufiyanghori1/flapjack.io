## Notification routing

The graphic below shows how events are turned into
notifications, and how notifications are routed to contacts as alerts. Further down, you
can find a step-by-step description of the process.

![notification routing](/images/notification-routing.gif)

## Details

1. **Receive events.**
    * Events are generated by external check execution systems and created as json objects in the `events` queue in Redis.
    * Processor removes events off the queue and processes them one by one.

2. **Determine when to generate a notification**. The event is discarded by the filters if ANY of the following are true:
    * event state is OK and it's the first event we've seen for this check (new check)
    * event state is OK and previous state for check was OK
    * check is in scheduled or unscheduled maintenance
    * check is failing and duration of current failure is less than the `initial_failure_delay` (default 30 seconds, can be specified in globally in config, set on a check or on the incoming event)
    * check is failing and time elapsed since last notification is less than `repeat_failure_delay` (default 60 sec, can be specified in globally in config, set on a check or on the incoming event)
    * check is recovering and time elapsed since last notification is less than `initial_recovery_delay` (default 0 sec (for backwards compatibility), can be specified in globally in config, set on a check or on the incoming event)

3. **Find all media that the check can alert on**
    1. For each medium, find all enabled rules that match the problem condition, time, and tags of the check.
        * Rules with a 'global' strategy match all checks.
        * Rules with an 'any_tag' strategy match if the rule has at least one tag in common with the check.
        * Rules with an 'all_tags' strategy match only if all of the rule's tags are set on the check.
        * Rules with a 'no_tag' strategy match only if the rule has no tags in common with the check.

    2. Don't send a notification on that medium if any of the matching rules has 'blackhole' set.

4. **Discard media based notification interval**.
    * If the check is failing, discard media to which an alert has been sent in the last media `interval` seconds. Each medium has its own value for `interval`, e.g., Ada may want to receive repeat alerts every 2 hours by SMS.

#### Creating the above graphic

1. Export [architecture diagrams](/images/FlapjackArchitecture.key) to PNG
2. Convert to gif using Acorn or similar
