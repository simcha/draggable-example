<template lang="pug">
  .fluid.container
    .row
      .col-6
        draggable.list-group(element="ul" v-model="list" :options="dragOptions1")
          transition-group(type="transition" :name="'flip-list'")
            li.list-group-item(v-for="element in list" :key="element.order")
              .person
                span.badge.float-left.badge-primary.m-2 {{element.order}}
                | {{element.name}}
      .col-6
        ul
          li.list-group-item(v-for="element in list2" :key="element.order")
            | {{element.name}}
            draggable(element="span" v-model="element.inlist" :options="dragOptions2" @add="onAdd")
              transition-group.list-group(name="no" tag="ul")
                li.list-group-item(v-for="subelement in element.inlist" :key="subelement.order")
                  span.badge.float-left.badge-info.m-2 {{subelement.order}}
                  span {{subelement.name}}
    .row
      .list-group.col-md-6
        pre.
          {{listNString}}
      .list-group.col-md-6
        pre.
          {{list2String}}
</template>

<script>
import draggable from 'vuedraggable'
const message = [ 'vue.draggable', 'draggable', 'component', 'for', 'vue.js 2.0', 'based' , 'on', 'Sortablejs' ]

export default {
  name: 'hello',
  components: {
    draggable,
  },
  data () {
    return {
      list: message.map( (name,index) => {return {name, order: index+1, fixed: false, person: true}; }),
      list2:[{ name: 'bielany', order: 20, fixed: true, inlist: []}, {name:'śródmieście', order: 21, fixed: true, bucket: true, inlist: []}],
      listn:[]
    }
  },
  methods:{
    orderList () {
      this.list = this.list.sort((one,two) =>{return one.order-two.order; })
    },
    onAdd (evnt) {
     window.test = (evnt)
    }
  },
  computed: {
    dragOptions1 () {
      return  {
        group: {name: 'description', put: 'false'},
        sort: false,
        handle:'.badge'
      };
    },
    dragOptions2 () {
      return  {
        group: 'description',
        sort: false,
        handle:'.badge'
      };
    },
    listNString(){
      return JSON.stringify(this.listn, null, 2);
    },
    list2String(){
      return JSON.stringify(this.list2, null, 2);
    }
  }
}
</script>

<style>

.no-move {
  transition: transform 0.5s;
}

.sortable-ghost {
  color: #0c0;
}
.sortable-chosen {
  color: #c00;
  background-color: #c00;
}

.sortable-drag {
  color: #00c;
  background-color: #c00;
}

.list-group {
  min-height: 20px;
}

.list-group-item .badge {
  cursor: move;
}

.list-group-item i{
  cursor: pointer;
}
</style>
