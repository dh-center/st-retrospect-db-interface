<template>
  <div class="entities-overview-create">
    <component
      :is="infoComponent"
      v-if="entity"
      ref="entityInfo"
      editable
      :entity="entity"
    />
    <button @click="saveEntity">
      Save
    </button>
  </div>
</template>

<script>
  /* eslint-disable new-cap */

  import axios from 'axios';
  import jsonpatch from 'fast-json-patch';

  export default {
    name: 'EntitiesOverviewSpecific',
    props: {
      model: {
        type: Function,
        required: true
      }
    },
    data() {
      return {
        entity: null,
        infoComponent: null
      };
    },
    async mounted() {
      await this.fetchData();
    },
    methods: {
      async fetchData() {
        this.infoComponent = (await import(`../${this.model.entityType}/Info`)).default;

        const { changeRecordId } = this.$route.params;

        const entityData = {};

        if (changeRecordId) {
          const changeRecord = await axios.get(`/changes/${this.model.entityType}/${changeRecordId}`);

          this.entity = new this.model(jsonpatch.applyPatch({}, changeRecord.changeList).newDocument);
        } else {
          this.entity = new this.model(entityData);
        }
      },
      async saveEntity() {
        if (this.$route.params.changeRecordId) {
          await axios.patch(`/changes/${this.model.entityType}/${this.$route.params.changeRecordId}`, this.entity.data);
        } else {
          const changesRecord = await axios.post(`/changes/${this.model.entityType}`, this.entity.data);

          this.$router.push({
            name: `${this.model.entityType}-create`,
            params: {
              changeRecordId: changesRecord._id
            }
          });
        }
      }
    }
  };
</script>