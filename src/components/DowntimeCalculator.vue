<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="6" offset="3">
        <v-text-field
          label="SLA"
          placeholder="SLA (e.g. 99.99)"
          suffix="%"
          v-model="sla"
          outlined
        ></v-text-field>
        <v-slider
          v-model="sla"
          max="99.99999"
          min="95"
          step="0.00001"
        ></v-slider>
      </v-col>
    </v-row>
    <v-row>
      <v-col  cols="6" offset="3">
        <v-card elevation="2" tile> 
          <v-card-title>Downtime per Day</v-card-title>
          <v-card-text>{{downtimePerDay}}</v-card-text>
        </v-card>
      </v-col>
    </v-row>
     <v-row>
      <v-col  cols="6" offset="3">
        <v-card elevation="2" tile> 
          <v-card-title>Downtime per Week</v-card-title>
          <v-card-text>{{downtimePerWeek}}</v-card-text>
        </v-card>
      </v-col>
    </v-row>
        <v-row>
      <v-col  cols="6" offset="3">
        <v-card elevation="2" tile> 
          <v-card-title>Downtime per Month</v-card-title>
          <v-card-text>{{downtimePerMonth}}</v-card-text>
        </v-card>
      </v-col>
    </v-row>
        <v-row>
      <v-col  cols="6" offset="3">
        <v-card elevation="2" tile> 
          <v-card-title>Downtime per Year</v-card-title>
          <v-card-text>{{downtimePerYear}}</v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import { Duration } from "luxon";
import { Component, Vue, Watch } from "vue-property-decorator";

@Component({})
export default class DowntimeCalculator extends Vue {
  public sla: number = 99.999;

  public get downtimePerDay(): string {
    const oneDayMilliseconds = Duration.fromObject({ days: 1 }).as(
      "milliseconds"
    );
    const upTimeMilliseconds = oneDayMilliseconds * (this.sla / 100);
    const downTimeDuration = Duration.fromObject({
      days: 0,
      hours: 0,
      minutes: 0,
      seconds: 0,
      milliseconds: oneDayMilliseconds - upTimeMilliseconds,
    });
    return this.formatDuration(downTimeDuration);
  }

  public get downtimePerWeek(): string {
    const totalMilliseconds = Duration.fromObject({ weeks: 1 }).as(
      "milliseconds"
    );
    const upTimeMilliseconds = totalMilliseconds * (this.sla / 100);
    const downTimeDuration = Duration.fromObject({
      days: 0,
      hours: 0,
      minutes: 0,
      seconds: 0,
      milliseconds: totalMilliseconds - upTimeMilliseconds,
    });
    return this.formatDuration(downTimeDuration);
  }

  public get downtimePerMonth(): string {
    const totalMilliseconds = Duration.fromObject({ month: 1 }).as(
     'milliseconds'
    );
    const upTimeMilliseconds = totalMilliseconds * (this.sla / 100);
    const downTimeDuration = Duration.fromObject({
      days: 0,
      hours: 0,
      minutes: 0,
      seconds: 0,
      milliseconds: totalMilliseconds - upTimeMilliseconds,
    });
    return this.formatDuration(downTimeDuration);
  }

  public get downtimePerQuarter(): string {
    const totalMilliseconds = Duration.fromObject({ month: 4 }).as(
    'milliseconds'
    );
    const upTimeMilliseconds = totalMilliseconds * (this.sla / 100);
    const downTimeDuration = Duration.fromObject({
      days: 0,
      hours: 0,
      minutes: 0,
      seconds: 0,
      milliseconds: totalMilliseconds - upTimeMilliseconds,
    });
    return this.formatDuration(downTimeDuration);
  }

  public get downtimePerYear(): string {
    const totalMilliseconds = Duration.fromObject({ year: 1 }).as(
      'milliseconds'
    );
    const upTimeMilliseconds = totalMilliseconds * (this.sla / 100);
    const downTimeDuration = Duration.fromObject({
      days: 0,
      hours: 0,
      minutes: 0,
      seconds: 0,
      milliseconds: totalMilliseconds - upTimeMilliseconds,
    });
    return this.formatDuration(downTimeDuration);
  }

  public formatDuration(duration: Duration): string {
    const durationObject = duration
      .shiftTo("days", "hours", "minutes", "seconds", "milliseconds")
      .toObject();

    const parts: string[] = [];
    if (durationObject.days && durationObject.days > 0) {
      parts.push(`${durationObject.days} days`);
    }
    if (durationObject.hours && durationObject.hours > 0) {
      parts.push(`${durationObject.hours} hours`);
    }
    if (durationObject.minutes && durationObject.minutes > 0) {
      parts.push(`${durationObject.minutes} minutes`);
    }
    if (durationObject.seconds && durationObject.seconds > 0) {
      parts.push(`${durationObject.seconds} seconds`);
    }
    if (durationObject.milliseconds && durationObject.milliseconds > 0) {
      if (parts.length > 0) {
        parts.push(`and ${durationObject.milliseconds.toFixed()} milliseconds`);
      } else {
        parts.push(`${durationObject.milliseconds.toFixed()} milliseconds`);
      }
    }
    return parts.join(', ');
  }
}
</script>
