<template>
  <div class="omegaup-course-details card">
    <div class="card-header" v-if="!update">
      <h3 class="card-title">{{ T.courseNew }}</h3>
    </div>
    <div class="card-body">
      <form class="form" data-course-form v-on:submit.prevent="onSubmit">
        <div class="row">
          <div class="form-group col-md-4">
            <label class="faux-label"
              >{{ T.wordsName }}
              <input
                class="form-control"
                v-bind:class="{ 'is-invalid': invalidParameterName === 'name' }"
                data-course-new-name
                type="text"
                v-model="name"
                required="required"
            /></label>
          </div>
          <div class="form-group col-md-4">
            <label class="faux-label"
              >{{ T.courseNewFormShortTitle_alias_ }}
              <font-awesome-icon
                v-bind:title="T.courseNewFormShortTitle_alias_Desc"
                icon="info-circle" />
              <input
                class="form-control"
                v-bind:class="{
                  'is-invalid': invalidParameterName === 'alias',
                }"
                type="text"
                data-course-new-alias
                v-bind:disabled="update"
                v-model="alias"
                required="required"
            /></label>
          </div>
          <div class="form-group col-md-4">
            <span class="faux-label"
              >{{ T.courseNewFormShowScoreboard }}
              <font-awesome-icon
                v-bind:title="T.courseNewFormShowScoreboardDesc"
                icon="info-circle"
              />
            </span>
            <omegaup-radio-switch
              v-bind:value.sync="showScoreboard"
              v-bind:selected-value="showScoreboard"
              name="show-scoreboard"
            ></omegaup-radio-switch>
          </div>
        </div>
        <div class="row">
          <div class="form-group col-md-4">
            <label class="faux-label"
              >{{ T.courseNewFormStartDate }}
              <font-awesome-icon
                v-bind:title="T.courseNewFormStartDateDesc"
                icon="info-circle" />
              <omegaup-datepicker v-model="startTime"></omegaup-datepicker
            ></label>
          </div>
          <div class="form-group col-md-4">
            <span class="faux-label"
              >{{ T.courseNewFormUnlimitedDuration }}
              <font-awesome-icon
                v-bind:title="T.courseNewFormUnlimitedDurationDesc"
                icon="info-circle"
              />
            </span>
            <omegaup-radio-switch
              v-bind:value.sync="unlimitedDuration"
              v-bind:selected-value="unlimitedDuration"
            ></omegaup-radio-switch>
          </div>
          <div class="form-group col-md-4">
            <label class="faux-label"
              >{{ T.courseNewFormEndDate }}
              <font-awesome-icon
                v-bind:title="T.courseNewFormEndDateDesc"
                icon="info-circle" />
              <omegaup-datepicker
                v-bind:enabled="!unlimitedDuration"
                v-model="finishTime"
                v-bind:is-invalid="invalidParameterName === 'finish_time'"
              ></omegaup-datepicker
            ></label>
          </div>
        </div>
        <div class="row">
          <div class="form-group col-md-4">
            <label class="faux-label"
              >{{ T.profileSchool }}
              <input
                autocomplete="off"
                class="form-control typeahead school"
                type="text"
                v-model="school_name"
                v-on:change="onChange" /><input
                class="school_id"
                type="hidden"
                v-model="school_id"
            /></label>
          </div>
          <div class="form-group col-md-4">
            <span class="faux-label"
              >{{ T.courseNewFormBasicInformationRequired }}
              <font-awesome-icon
                v-bind:title="T.courseNewFormBasicInformationRequiredDesc"
                icon="info-circle"
              />
            </span>
            <omegaup-radio-switch
              v-bind:value.sync="needsBasicInformation"
              v-bind:selected-value="needsBasicInformation"
            ></omegaup-radio-switch>
          </div>
          <div class="form-group col-md-4">
            <span class="faux-label"
              >{{ T.courseNewFormUserInformationRequired }}
              <font-awesome-icon
                v-bind:title="T.courseNewFormUserInformationRequiredDesc"
                icon="info-circle"
              />
            </span>
            <select class="form-control" v-model="requests_user_information">
              <option value="no">
                {{ T.wordsNo }}
              </option>
              <option value="optional">
                {{ T.wordsOptional }}
              </option>
              <option value="required">
                {{ T.wordsRequired }}
              </option>
            </select>
          </div>
          <div class="form-group container-fluid">
            <label
              >{{ T.courseNewFormDescription }}
              <textarea
                class="form-control"
                v-bind:class="{
                  'is-invalid': invalidParameterName === 'description',
                }"
                cols="30"
                rows="5"
                v-model="description"
                required="required"
              ></textarea>
            </label>
          </div>
          <div class="form-group col-md-12 text-right">
            <button class="btn btn-primary mr-2 submit" type="submit">
              <template v-if="update">
                {{ T.courseNewFormUpdateCourse }}
              </template>
              <template v-else>
                {{ T.courseNewFormScheduleCourse }}
              </template>
            </button>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.omegaup-course-details .form-group > label {
  width: 100%;
}
.omegaup-course-details .faux-label {
  font-weight: bold;
}
</style>

<script lang="ts">
import { Vue, Component, Prop, Watch, Emit } from 'vue-property-decorator';
import { types } from '../../api_types';
import T from '../../lang';
import * as typeahead from '../../typeahead';
import DatePicker from '../DatePicker.vue';
import omegaup_RadioSwitch from '../RadioSwitch.vue';

import {
  FontAwesomeIcon,
  FontAwesomeLayers,
  FontAwesomeLayersText,
} from '@fortawesome/vue-fontawesome';
import { fas } from '@fortawesome/free-solid-svg-icons';
import { library } from '@fortawesome/fontawesome-svg-core';
library.add(fas);

@Component({
  components: {
    'omegaup-datepicker': DatePicker,
    'omegaup-radio-switch': omegaup_RadioSwitch,
    'font-awesome-icon': FontAwesomeIcon,
    'font-awesome-layers': FontAwesomeLayers,
    'font-awesome-layers-text': FontAwesomeLayersText,
  },
})
export default class CourseDetails extends Vue {
  @Prop({ default: false }) update!: boolean;
  @Prop() course!: types.CourseDetails;
  @Prop({ default: '' }) invalidParameterName!: string;

  T = T;
  alias = this.course.alias;
  description = this.course.description;
  finishTime = this.course.finish_time || new Date();
  showScoreboard = this.course.show_scoreboard;
  startTime = this.course.start_time;
  name = this.course.name;
  school_name = this.course.school_name;
  school_id = this.course.school_id;
  needsBasicInformation = this.course.needs_basic_information;
  requests_user_information = this.course.requests_user_information;
  unlimitedDuration = this.course.finish_time === null;

  data(): { [name: string]: any } {
    return {
      school_id: this.course.school_id,
    };
  }

  mounted(): void {
    typeahead.schoolTypeahead(
      $('input.typeahead', this.$el),
      (event: Event, item: any) => {
        this.school_name = item.value;
        this.school_id = item.id;
      },
    );
  }

  reset(): void {
    this.alias = this.course.alias;
    this.description = this.course.description;
    this.finishTime = this.course.finish_time || new Date();
    this.showScoreboard = this.course.show_scoreboard;
    this.startTime = this.course.start_time;
    this.name = this.course.name;
    this.school_name = this.course.school_name;
    this.school_id = this.course.school_id;
    this.needsBasicInformation = this.course.needs_basic_information;
    this.requests_user_information = this.course.requests_user_information;
    this.unlimitedDuration = this.course.finish_time === null;
  }

  onSubmit(): void {
    this.$emit('submit', this);
  }

  @Emit('emit-cancel')
  onCancel(): void {
    this.reset();
  }

  onChange(): void {
    if (this.course.school_id === this.school_id) {
      this.school_id = undefined;
    } else {
      this.course.school_id = this.school_id;
    }
  }
}
</script>
