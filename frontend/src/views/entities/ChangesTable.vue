<template>
  <table class="data-table">
    <thead>
      <tr>
        <th>№</th>
        <th>{{ $t('entities.actions') }}</th>
        <th>{{ $t('entities.changesAuthor') }}</th>
        <th
          v-for="(fieldName, index) in model.fields"
          :key="index"
        >
          {{ $t(`${model.entityType}.${fieldName}`) }}
        </th>
      </tr>
    </thead>
    <tbody>
      <ChangesTableRow
        v-for="(changeRecord, index) in changesRecordList"
        :key="changeRecord._id"
        :row-index="index + 1"
        :model="model"
        :change-record="changeRecord"
        @successfulApprove="deleteChangeRecord(changeRecord)"
      />
    </tbody>
  </table>
</template>

<script>
  import axios from 'axios';
  import ChangesTableRow from '../../components/ChangesTableRow';

  export default {
    name: 'ChangesTable',
    components: {
      ChangesTableRow
    },
    props: {
      model: {
        type: Function,
        required: true
      }
    },
    data() {
      return {
        changesRecordList: []
      };
    },
    async created() {
      this.changesRecordList = await axios.get(`/changes/${this.model.entityType}`);
    },
    methods: {
      async deleteChangeRecord(changeRecord) {
        this.$delete(this.changesRecordList, this.changesRecordList.indexOf(changeRecord));
      }
    }
  };
</script>

<style src="../../styles/data-table.css"></style>
