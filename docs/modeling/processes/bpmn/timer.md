---
Title: Timer events
---

# Timer events
Timer events are used to influence events at specific times, after a set amount of time has passed or at intervals. All timer events use the international standard [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) for specifying time formats. 

Timer events are graphically represented by a clock icon inside different shapes that differentiate between the event types.

The following are timer events: 

* [Timer start events](../bpmn/start.md#timer-start-events)
* [Timer boundary events](../bpmn/boundary.md#timer-boundary-events)
* [Timer intermediate catch events](#timer-intermediate-catch-events)

All timer events require either a [`timeDate`](#timedate), [`timeDuration`](#timeduration) or [`timeCycle`](#timecycle) property included within the `timerEventDefinition` to define the timer trigger behaviour.

## timeDate
The `timeDate` property for timer events defines a specific date and time in ISO 8601 format for when the trigger will be fired and can include a specified time zone. 

The following is an example of the `timerEventDefinition` using a `timeDate`: 

* `2017-05-17` represents the 17th May 2017 in *YYYY-MM-DD* format
* `T12:42:23` represents the time of 12:42:23 in *hh:mm:ss* format
* `Z` represents that the time format is in UTC (Coordinated Universal Time). The time can also contain UTC offsets such as `+01` for an hour ahead of UTC. When an offset is defined the `Z` is not required, for example: `T12:42:23+01`

```xml
<bpmn2:timerEventDefinition> 
  <bpmn2:timeDate xsi:type="bpmn2:tFormalExpression">2017-05-17T12:42:23Z</bpmn2:timeDate>
</bpmn2:timerEventDefinition>
```

## timeDuration
The `timeDuration` property for timer events defines how long a timer should wait in ISO 8601 format before the trigger is fired. 

The following are the letters used to refer to duration:

| Letter | Description | Example | 
| ------ | ----------- | ------- |
| `P` | Designates that the following letters and numbers represent a duration. Must always be present | |
| `Y` | Represents a year and follows the number of years | `P2Y` for 2 years |
| `M` | Represents a month and follows the number of months when preceded by a `P` | `P3Y4M` for 2 years and 4 months |
| `W` | Represents a week and follows the number of weeks | `P10W` for 10 weeks |
| `D` | Represents a day and follows the number of days | `P1Y1M1D` for 1 year, 1 month and 1 day |
| `T` | Designates that the following letters represent a duration of hours to seconds. Must always be present to refer to hours, minutes and seconds | 
| `H` | Represents an hour and follows the number of hours | `P1DT0.5H` for 1 day and half an hour |
| `M` | Represents a minute and follows the number of minutes when preceded by a `T` | `PT1M` for 1 minute |
| `S` | Represents a second and follows the number of seconds | `P2Y3M4DT5H6M7S` for 2 years, 3 months, 4 days, 5 hours, 6 minutes and 7 seconds |

The following is an example of the `timerEventDefinition` using a `timeDuration` to represent a duration of 5 days: 

```xml
<bpmn2:timerEventDefinition>
  <bpmn2:timeDuration xsi:type="bpmn2:tFormalExpression">P5D</bpmn2:timeDuration>
</bpmn2:timerEventDefinition>
```

## timeCycle
The `timeCycle` property for timer events defines intervals for the trigger to fire at. Intervals can be defined using the [time intervals](#time-intervals) that adhere to the ISO 8601 standard or by using [cron expressions](#cron-expression-intervals).

### Time intervals
Time intervals use the syntax `R/` to set a number of repetitions, for example `R5/` would repeat five times. 

Following the repetition, a duration can be set for when the repetition occurs, for example `R5/PT10H` would repeat every 10 hours, five times. 

**Note** The duration uses the same format as for [`timeDuration`](#timeduration).

An optional end date can also be set after the duration and separated by a `/`. 

**Note**: The end date uses the same format as for [`timeDate`](#timedate).   

The following is an example of the `timerEventDefinition` using time interval syntax for `timeCycle` to represent three repetitions every 30 minutes:

``` xml
<bpmn2:timerEventDefinition>
  <bpmn2:timeCycle xsi:type="bpmn2:tFormalExpression">R3/PT30M</bpmn2:timeCycle>
</bpmn2:timerEventDefinition> 
```

### Cron expression intervals
[Cron expressions](https://en.wikipedia.org/wiki/Cron#CRON_expression) can be used to define repeating triggers for timer events.  

The following is an example of the `timerEventDefinition` using a cron expression for `timeCycle` to represent a trigger firing every 5 minutes beginning at the top of the hour:

```xml
<bpmn2:timerEventDefinition>
  <bpmn2:timeCycle>0 0/5 * * * ?</bpmn2:timeCycle>
</bpmn2:timerEventDefinition>
```

## Timer expressions
All properties within `timerEventDefinition` can accept process variables as their values as long as they are in ISO 8601 or cron expression format.

The following is an example of the process variable `{$duration}` being used: 

```xml
<bpmn2:timerEventDefinition>
	<bpmn2:timeDuration xsi:type="bpmn2:tFormalExpression">${duration}</bpmn2:timeDuration>
</bpmn2:timerEventDefinition>
```

## Timer intermediate catch events
Timer intermediate catching events cause the process flow to wait until a specific time or interval is reached. The time to wait is defined in the `timerEventDefinition` property. 

Timer intermediate catching events are graphically represented by two thin concentric circles with a clock icon inside.

The XML representation of timer intermediate catching events is:

```xml
<bpmn2:intermediateCatchEvent id="IntermediateCatchEvent5">
	<bpmn2:incoming>SequenceFlow_3</bpmn2:incoming>
	<bpmn2:outgoing>SequenceFlow_4</bpmn2:outgoing>
	<bpmn2:timerEventDefinition>
  		<bpmn2:timeDuration xsi:type="bpmn2:tFormalExpression">P5D</bpmn2:timeDuration>
	</bpmn2:timerEventDefinition>
</bpmn2:intermediateCatchEvent>
```

**Note**: This will wait five days before continuing the process. 
