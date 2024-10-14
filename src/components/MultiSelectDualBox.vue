<template>
    <div class="row">
        <div class="col col-4">
            <VueDraggable class="list-group" v-model="list1" group="listgroup">
                <div v-for="element in list1" :key="element.id" class="item">
                    <div class="list-group-item" v-on:click="toggleActive('list1', element.id)" :class="{
                        active:
                            list1Selected.find((item) => item === element.id) !==
                            undefined,
                    }">
                        {{ element.name }}
                    </div>
                </div>
            </VueDraggable>
        </div>
        <div class="col col-4 center-content">
            <v-btn icon flat class="customButton" color="#454545" @click="select"
                :disabled="list1Selected.length === 0">
                <!-- <v-icon>mdi-arrow-right</v-icon> -->
            </v-btn>
            <v-spacer />
            <button class="customButton" color="#454545" @click="unselect" :disabled="list2Selected.length === 0"
                text="Arrow Left">
                Arrow Left
            </button>
        </div>
        <div class="col col-4">
            <VueDraggable class="list-group" v-model="list2" group="listgroup">
                <div v-for="element in list2" :key="element.id" class="item">
                    <div class="list-group-item" v-on:click="toggleActive('list2', element.id)" :class="{
                        active:
                            list2Selected.find((item) => item === element.id) !==
                            undefined,
                    }">
                        {{ element.name }}
                    </div>
                </div>
            </VueDraggable>
        </div>
    </div>
</template>

<script>
import { defineComponent } from "vue";
// import draggable from "vuedraggable";
import { VueDraggable } from 'vue-draggable-plus'
import {
    ChevronBack as ArrLeft,
    ChevronForward as ArrRight,
} from "@vicons/ionicons5";

export default defineComponent({
    name: "transition-list",
    display: "Transition",
    order: 6,
    setup() {
        //let dataSetStore = dataStore();
    },
    components: {
        // draggable,
        VueDraggable,
        ArrLeft,
        ArrRight,
    },
    data() {
        const available_col = [
            {
                name: 'Joao',
                id: 1,
            },
            {
                name: 'Jean',
                id: 2,
            },
            {
                name: 'Johanna',
                id: 3,
            },
            {
                name: 'Juan',
                id: 4,
            },
        ];
        //if(this.viewDataSetStore.selectedColumns.length === 0) {
        // const available_col_mod = this.mapColumns(available_col);


        //}
        const selected_col = [];
        // const selected_col_mod = this.mapColumns(selected_col);
        //debugger;
        return {
            list1: available_col,
            list2: [],
            list1Selected: [],
            list2Selected: [],
        };
    },
    watch: {
        selectedColumns(newVal) {
            const selected_col = newVal;
            const selected_col_mod = this.mapColumns(selected_col);
            this.list2 = selected_col_mod;
        },
        availableColumns(newVal) {
            // let available_col = newVal.filter(
            //     (obj) => !this.dataSetStore.permanentlyHiddenColumns.includes(obj.id)
            // );
            // const available_col_mod = this.mapColumns(available_col);
            // this.list1 = available_col_mod;
            //debugger;
        },
    },
    methods: {
        mapColumns(columns) {
            const columns_mod = [];
            for (const item of columns) {
                if (item.column_id === undefined && item.id !== undefined) {
                    item.column_id = item.id;
                }
                if (item.name !== undefined) {
                    item.column_name = item.name;
                }
                columns_mod.push(item);
            }
            return columns_mod;
        },
        toggleActive(list, index) {
            if (list === "list1") {
                //debugger;
                const item = this.list1Selected.find((item) => item === index);
                if (item == null) {
                    this.list1Selected.push(index);
                } else {
                    const itemIndex = this.list1Selected
                        .map((item) => item)
                        .indexOf(index);
                    this.list1Selected.splice(itemIndex, 1);
                }

                this.list2Selected = [];
                return;
            }
            const item = this.list2Selected.find((item) => item === index);
            if (item == null) {
                this.list2Selected.push(index);
            } else {
                const itemIndex = this.list2Selected.map((item) => item).indexOf(index);
                this.list2Selected.splice(itemIndex, 1);
            }
            this.list1Selected = [];
        },
        select() {
            for (const key in this.list1Selected) {
                const list1SelectedItem = this.list1Selected[key];
                const item = this.list1.find(
                    (item) => item.column_id === parseInt(list1SelectedItem)
                );
                const itemIndex = this.list1
                    .map((item) => item.column_id)
                    .indexOf(list1SelectedItem);
                if (item !== undefined && itemIndex >= 0) {
                    // this.list2.push({
                    //   column_id: item.column_id,
                    //   column_index: item.column_index,
                    //   column_name: item.column_name,
                    // });
                    this.list2.push(item);
                    this.list1.splice(itemIndex, 1);
                }
            }

            this.list1Selected = [];
            this.list2Selected = [];
        },
        unselect() {
            for (const key in this.list2Selected) {
                const list2SelectedItem = this.list2Selected[key];
                const item = this.list2.find(
                    (item) => item.column_id === list2SelectedItem
                );
                const itemIndex = this.list2
                    .map((item) => item.column_id)
                    .indexOf(list2SelectedItem);
                // this.list1.push({
                //   column_id: item.column_id,
                //   column_index: item.column_index,
                //   column_name: item.column_name,
                // });
                this.list1.push(item);
                this.list2.splice(itemIndex, 1);
            }
            this.list2Selected = [];
            this.list1Selected = [];
        },
    },
});
</script>

<style scoped>
.customButton {
    color: white;
    text-transform: none;
    letter-spacing: normal;
    font-weight: normal;
}

.customButton:focus {
    color: white;
    background-color: #2a538e;
}

.flip-list-move {
    transition: transform 0.5s;
}

.no-move {
    transition: transform 0s;
}

.ghost {
    opacity: 0.5;
    background: #c8ebfb;
}

.list-group {
    list-style: none;
    border: 1px solid #252b45;
    padding: -1px;
    height: 300px;
    overflow-y: auto;
    overflow-x: hidden;
}

.list-group span {
    display: inline-block;
    height: 100%;
    width: 100%;
}

.list-group-item {
    cursor: move;
    font-size: 0.9em;
    color: #2c3e50;
    border: 1px solid #252b45;
    padding: 5px 10px;
    background: #fff;
    width: calc(100% + 2px);
    margin: -1px;
    font-family: "inter", sans-serif;
}

.list-group-item.active {
    background: #003479;
    color: #fff;
}

.btn-wrap {
    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
    align-items: center;
}

.row {
    display: flex;
    flex-wrap: wrap;
    margin-right: -15px;
    margin-left: -15px;
}

.col {
    padding-right: 15px;
    padding-left: 15px;
}

.col-4 {
    flex: 0 0 50.3333%;
    max-width: 50.3333%;
}

.center-content {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

.no-gutters {
    margin-right: 0;
    margin-left: 0;
}

.no-gutters .col {
    padding-right: 0;
    padding-left: 0;
}
</style>
