<template>
  <div class="select-wrapper">
    <div class="selection">
      <!--Todo:  ==========assign Table ListBox========== -->
      <table class="table-scroll small-first-col">
        <thead>
          <tr>
            <th style="text-align: center">Assign</th>
          </tr>
        </thead>
        <tbody class="body-half-screen">
          <tr>
            <td>
              <el-tree :data="data" @dblclick="doubleClickNode()" @node-click="handleNodeClick" />
            </td>
          </tr>
        </tbody>
      </table>
      <!-- <div class="button-wraper">
        <div class="button-transfer">
          <button>>></button>
          <button>></button>
          <button>></button>
          <button>one</button>
        </div>
      </div> -->
      <!--Todo: ==========Unassign Table ListBox==========  -->
      <table class="table-scroll small-first-col">
        <thead>
          <tr>
            <th style="text-align: center">Unassign</th>
          </tr>
        </thead>
        <tbody class="body-half-screen">
          <td>
            <el-tree :data="unassignBox" />
          </td>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import Tree from '@/Types/Tree';
export default defineComponent({
  components: {},

  setup() {
    const value = ref([]);
    const selectedTree = ref();
    const data = ref<Tree[]>([
      {
        label: 'FRUIT',
        children: [
          {
            label: 'Banana',
          },
          {
            label: 'Coconut',
          },
          // {
          //   label: 'Grape',
          // },
          // {
          //   label: 'Mango',
          // },
          // {
          //   label: 'Beans',
          // },
          // {
          //   label: 'Apple',
          // },
          // {
          //   label: 'Orange',
          // },
          // {
          //   label: 'PineApple',
          // },
          // {
          //   label: 'Lemon',
          // },
          // {
          //   label: 'Blue Berry',
          // },
          // {
          //   label: 'Red Buerry',
          // },
        ],
      },
      {
        label: 'ANIMAL',
        children: [
          {
            label: 'Dog',
          },
          {
            label: 'Cat',
          },
          {
            label: 'Dragon',
          },
          {
            label: 'Ant',
          },
          {
            label: 'Bat',
          },
          {
            label: 'Bear',
          },
          {
            label: 'Bee',
          },
          {
            label: 'Bird',
          },
          {
            label: 'Buffalo',
          },
          {
            label: 'Chicken',
          },
          {
            label: 'Cow',
          },
          {
            label: 'Duck',
          },
          {
            label: 'Elephant',
          },
          {
            label: 'Fish',
          },
          {
            label: 'Fox',
          },
          {
            label: 'Frog',
          },
          {
            label: 'Goose',
          },
          {
            label: 'Goat',
          },
        ],
      },
      {
        label: 'FOOD',
        children: [
          {
            label: 'Pizza',
          },
          {
            label: 'Pasta',
          },
          {
            label: 'Donuts',
          },
          {
            label: 'Bread',
          },
          {
            label: 'Soup',
          },
          {
            label: 'Cake',
          },
          {
            label: 'Pan-Cake',
          },
          {
            label: 'Candy',
          },
          {
            label: 'Jam',
          },
          {
            label: 'Hamberger',
          },
          {
            label: 'Coca',
          },
          {
            label: 'Green-Tea',
          },
          {
            label: 'Chocolate-Frappe',
          },
          {
            label: 'Mocha-Frappe',
          },
          {
            label: 'Ice-Latte',
          },
          {
            label: 'Hot-Latte',
          },
          {
            label: 'Salmon',
          },
          {
            label: 'Ice-Cream',
          },
          {
            label: 'Cherry-Pie',
          },
          {
            label: 'Chinese Food',
          },
          {
            label: 'Spaghetti',
          },
        ],
      },
      {
        label: 'CAR',
        children: [
          {
            label: 'Ford',
          },
          {
            label: 'Range',
          },
          {
            label: 'GTR',
          },
          {
            label: 'Tesla',
          },
          {
            label: 'BMW',
          },
          {
            label: 'Ferrari',
          },
          {
            label: 'Porshe',
          },
          {
            label: 'Honda',
          },
          {
            label: 'Lamborghini',
          },
          {
            label: 'Toyota',
          },
          {
            label: 'Bentley',
          },
          {
            label: 'Maserati',
          },
          {
            label: 'Audi',
          },
          {
            label: 'Jeep',
          },
          {
            label: 'Subaru',
          },
          {
            label: 'Cadilac',
          },
          {
            label: 'Chrysler',
          },
          {
            label: 'Mazda',
          },
          {
            label: 'Chevrolet-Corvette',
          },
          {
            label: 'Dodge',
          },
          {
            label: 'Hyundai',
          },
        ],
      },
      {
        label: 'Movie',
        children: [
          {
            label: 'Ninja-Turtle',
          },
          {
            label: 'Shang-Chi',
          },
          {
            label: 'Spider-Man',
          },
          {
            label: 'Bad-Man',
          },
          {
            label: 'Avenger-Endgame',
          },
          {
            label: 'Ant-Man',
          },
          {
            label: 'Ninja-Go',
          },
          {
            label: 'Black-Panther',
          },
          {
            label: 'Spider-Man',
          },
          {
            label: 'Luca-Alberto',
          },
          {
            label: 'E-Cando',
          },
          {
            label: 'Kongfu-Panda',
          },
          {
            label: 'Super-Man',
          },
          {
            label: 'Iron-Man',
          },
          {
            label: 'Power-Ranger',
          },
          {
            label: 'Ben-Ten',
          },
          {
            label: 'Dead-Pool',
          },
          {
            label: 'Red-Bear',
          },
        ],
      },
    ]);
    const unassignBox = ref<Tree[]>([]);

    const handleNodeClick = (data: Tree) => {
      selectedTree.value = data;
    };
    const doubleClickNode = () => {
      doubleClickParentNode();
      doubleClickSingleNode();
    };

    const doubleClickParentNode = () => {
      let target = JSON.parse(JSON.stringify(selectedTree.value));
      let items: Tree;
      if (!matchParent(target.label)) {
        data.value.filter((myData) => {
          if (myData.label === target.label) {
            items = JSON.parse(JSON.stringify(myData));
            unassignBox.value.push(items);

            let indexOfParent: number;
            for (let index = 0; index < data.value.length; index++) {
              const element = data.value[index];
              if (element.label == myData.label) {
                indexOfParent = index;
                data.value.splice(indexOfParent, 1);
              }
            }
          }
        });
      } else {
        let childData: Tree;
        data.value.filter((child) => {
          if (child.label === target.label) {
            items = JSON.parse(JSON.stringify(child));
            childData = items;

            let indexOfParent: number;
            for (let index = 0; index < data.value.length; index++) {
              const element = data.value[index];
              if (element.label == child.label) {
                indexOfParent = index;
                data.value.splice(indexOfParent, 1);
              }
            }
          }
        });

        unassignBox.value.filter((find) => {
          if (find.label === target.label) {
            childData.children?.forEach((currentChilds) => {
              find.children?.push(currentChilds);
            });
          }
        });
      }
    };

    const doubleClickSingleNode = () => {
      let target = JSON.parse(JSON.stringify(selectedTree.value));
      let items: Tree;

      data.value.filter((myData) => {
        myData.children?.filter((list) => {
          if (list.label === target.label) {
            items = JSON.parse(JSON.stringify(myData));
            items.children = [];
            items.children?.push(target);

            if (matchParent(items.label)) {
              unassignBox.value.filter((match) => {
                if (match.label === items.label) {
                  match.children?.push(list);
                }
              });
            } else {
              unassignBox.value.push(items);
            }
            myData.children = myData.children?.filter((found) => {
              return found.label !== target.label;
            });

            if (myData.children?.length == 0) {
              let indexOfParent: number;
              for (let index = 0; index < data.value.length; index++) {
                const element = data.value[index];
                if (element.label == myData.label) {
                  indexOfParent = index;
                  data.value.splice(indexOfParent, 1);
                }
              }
            }
          }
        });
      });
    };
    const matchParent = (parentName: string) => {
      const filterResult = unassignBox.value.filter((value) => value.label === parentName);
      if (filterResult.length !== 0) {
        return true;
      } else {
        return false;
      }
    };

    return {
      selectedTree,
      value,
      unassignBox,
      data,
      handleNodeClick,
      doubleClickNode,
      doubleClickParentNode,
      doubleClickSingleNode,
    };
  },
});
</script>

<style>
.select-wrapper {
  width: 1200px;
  margin: auto;
  height: 500px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.selection {
  width: 1200px;
  margin: auto;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.Select-option {
  position: relative;
  top: -130px;
  left: -40px;
}
.select-list {
  width: 300px;
  height: 200px;
  padding: 20px;
  height: 200px;
  width: 500px;
  padding: 10px;
  border-radius: 10px;
  background: #e0e0e0;
  box-shadow: inset 20px 20px 18px #cecece, inset -20px -20px 18px #f2f2f2;
  border: 2px solid rgb(179, 169, 169);
  outline: none;
  cursor: grab;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
* {
  margin: 0px;
  padding: 0px;
}
header {
  width: 100%;
  background-color: blue;
  height: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: whitesmoke;
  margin-bottom: 50px;
}
.subject-info-box-1,
.subject-info-box-2 {
  float: left;
  width: 45%;
}
.subject-info-box-1,
.subject-info-box-2,
h1 {
  margin-bottom: 20px;
  font-family: serif;
}

.select-list::-webkit-scrollbar {
  border-radius: 80px;
  background: #e0e0e0;
}
.select-list::-webkit-scrollbar-thumb {
  border-radius: 80px;
  background-color: rgb(179, 169, 169);
  border: 2px solid rgb(179, 169, 169);
}

option:hover {
  background: #eeeeee;
}
option {
  padding: 10px;
}
.subject-info-arrows {
  float: left;
  width: 10%;
  margin-top: 35px;
  position: relative;
  top: -70px;
}

input {
  width: 70%;
  position: relative;
  top: 55px;
  margin-bottom: 15px;
  border-radius: 44px;
  background: linear-gradient(145deg, #f0f0f0, #cacaca);
  box-shadow: 2px 5px 8px #aaaaaa, -2px -5px 8px #ffffff;
  border: 2px solid rgb(179, 169, 169);
  font-size: 20px;
  cursor: grab;
}
.container {
  padding: 1rem;
  margin: 1rem;
}
.table-scroll {
  /*width:100%; */
  display: block;
  empty-cells: show;

  /* Decoration */
  border-spacing: 0;
  border: 1px solid;
}

.table-scroll thead {
  background-color: #f1f1f1;
  position: relative;
  display: block;
  width: 100%;
  overflow-y: scroll;
}

.table-scroll tbody {
  /* Position */
  display: block;
  position: relative;
  width: 500px;
  height: 150px;
  overflow-y: scroll;
  /* Decoration */
  border-top: 1px solid rgba(0, 0, 0, 0.2);
}

.table-scroll tr {
  width: 100%;
  display: flex;
}

.table-scroll td,
.table-scroll th {
  flex-basis: 100%;
  flex-grow: 2;
  display: block;
  padding: 1rem;
  text-align: left;
}

/* Other options */

.table-scroll.small-first-col td:first-child,
.table-scroll.small-first-col th:first-child {
  flex-basis: 20%;
  flex-grow: 1;
}
.body-half-screen {
  max-height: 50vh;
}

.small-col {
  flex-basis: 10%;
}

#unassign-td {
  font-size: 13px;
  color: rgb(109, 105, 105);
}
</style>
