<template>
  <div class="panel panel-primary">
    <div class="panel-body">
      <form class="clone_contest_form" v-on:submit.prevent="onSubmit">
        <div class="row">
          <div class="form-group col-md-6">
            <label>{{ T.wordsTitle }}</label>
            <input class="form-control" size="30" type="text" v-model="title" />
          </div>
          <div class="form-group col-md-6">
            <label>{{ T.contestNewFormShortTitle_alias_ }}</label>
            <span
              aria-hidden="true"
              class="glyphicon glyphicon-info-sign"
              data-placement="top"
              data-toggle="tooltip"
              title="T.contestNewFormShortTitle_alias_Desc"
            ></span>
            <input class="form-control" type="text" v-model="alias" />
          </div>
        </div>
        <div class="row">
          <div class="form-group col-md-6">
            <label>{{ T.contestNewFormDescription }}</label>
            <textarea
              class="form-control"
              cols="30"
              rows="10"
              v-model="description"
            ></textarea>
          </div>
          <div class="form-group col-md-3">
            <label>{{ T.contestNewFormStartDate }}</label>
            <span
              aria-hidden="true"
              class="glyphicon glyphicon-info-sign"
              data-placement="top"
              data-toggle="tooltip"
              title="T.contestNewFormStartDateDesc"
            ></span>
            <omegaup-datetime v-model="startTime"></omegaup-datetime>
          </div>
        </div>
        <div class="form-group">
          <button class="btn btn-primary" type="submit">
            {{ T.wordsCloneContest }}
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator';
import T from '../../lang';
import * as ui from '../../ui';
import DateTime from '../DateTimePicker.vue';

@Component({
  components: {
    'omegaup-datetime': DateTime,
  },
})
export default class Clone extends Vue {
  T = T;
  title = '';
  alias = '';
  description = '';
  startTime = new Date();

  onSubmit() {
    this.$emit('emit-clone', this);
  }
}
</script>
