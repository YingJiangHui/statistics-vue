<template>
    <Layout classPrefix="money">
        <NumberPad :value.sync="record.amount" @submit="saveRecord"/>
        <Tabs :value.sync="record.type" classPrefix="type" :list="recordTypeList"/>
        <div class="notes">
            <FormItem field-text="备注" placeholder="在这里添加备注" :value.sync="record.notes"/>
        </div>
        <div class="createAt">
            <FormItem field-text="日期"  :value.sync="record.createdAt" type="datetime-local"/>
        </div>

        <Tags @update:value="update"/>
    </Layout>
</template>

<script lang="ts">
  import Vue from "vue";
  import FormItem from '@/components/Money/FormItem.vue';
  import NumberPad from '@/components/Money/NumberPad.vue';
  import Tags from '@/components/Money/Tags.vue';
  import {Component} from "vue-property-decorator";
  import Tabs from '@/components/Tabs.vue';
  import recordTypeList from '@/constants/recordTypeList'


  @Component({
    components: {
      FormItem,
      NumberPad,
      Tags,
      Tabs
    }
  })

  export default class Money extends Vue {
    recordTypeList: DataSourceItem[] = recordTypeList as DataSourceItem[];
    recordList: RecordItem[] = this.$store.state.recordList;
    record: RecordItem = {
      tags: [],
      notes: '',
      type: '-',
      amount: 0,
      createdAt:new Date().toISOString()
    };

    update(value: string[]) {
      this.record.tags = value;
    }

    saveRecord() {
      if(this.record.tags.length===0){
        alert('至少添加一個標簽')
      }else{
        this.$store.commit('createRecords', this.record);
        this.record.notes = ''
        alert('成功提交')
      }
    }
  }

</script>


<style lang="scss">
    .money-content {
        display: flex;
        flex-direction: column-reverse;

    }

    .notes {
        > {
            background: rgb(245, 245, 245);
        }

        padding: 10px 0;
    }


</style>
<style lang="scss" scoped>
    ::v-deep {
        .type-tabs{
            .type-item{
                line-height: 48px;

            }
            >.selected {
               background: #ffb200;
            }
        }
    }
</style>