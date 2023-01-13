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
              <el-tree :data="data" @dblclick="doubleClickAssignNode()" @node-click="handleNodeClick" />
            </td>
          </tr>
        </tbody>
      </table>

      <!--Todo: ========== Button Transfer ==========  -->
      <div class="button-wraper">
        <div class="button-transfer">
          <button class="button" @click="btnTransferAllParentAssignNode()">
            <i class="fa-solid fa-angles-right"></i>
          </button>
          <button class="button" @click="singleTransferAssignBtn()">
            <i class="fa-solid fa-angle-right"></i>
          </button>
          <button class="button" @click="singleTransferUnassignBtn()">
            <i class="fa-solid fa-angle-left"></i>
          </button>
          <button class="button" @click="btnTransferAllParentUnAssignNode()">
            <i class="fa-solid fa-angles-left"></i>
          </button>
        </div>
      </div>

      <!--Todo: ==========Unassign Table ListBox==========  -->
      <table class="table-scroll small-first-col">
        <thead>
          <tr>
            <th style="text-align: center">Unassign</th>
          </tr>
        </thead>
        <tbody class="body-half-screen">
          <td>
            <el-tree :data="unassignBox" @dblclick="doubleClickUnssignNode()" @node-click="handleNodeClick" />
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
            label: '1.Banana',
          },
          {
            label: '2.Coconut',
          },
          {
            label: '3.Grape',
          },
          {
            label: '4.Mango',
          },
          {
            label: '5.Beans',
          },
          {
            label: '6.Apple',
          },
          {
            label: '7.Orange',
          },
        ],
      },
      {
        label: 'ANIMAL',
        children: [
          {
            label: '1.Dog',
          },
          {
            label: '2.Cat',
          },
          {
            label: '3.Dragon',
          },
          {
            label: '4.Ant',
          },
          {
            label: '5.Bat',
          },
          {
            label: '6.Bear',
          },
          {
            label: '7.Bee',
          },
        ],
      },
      {
        label: 'FOOD',
        children: [
          {
            label: '1.Pizza',
          },
          {
            label: '2.Pasta',
          },
          {
            label: '3.Donuts',
          },
          {
            label: '4.Bread',
          },
          {
            label: '5.Soup',
          },
          {
            label: '6.Cake',
          },
          {
            label: '7.Pan-Cake',
          },
        ],
      },
      {
        label: 'CAR',
        children: [
          {
            label: '1.Ford',
          },
          {
            label: '2.Range',
          },
          {
            label: '3.GTR',
          },
          {
            label: '4.Tesla',
          },
          {
            label: '5.BMW',
          },
          {
            label: '6.Ferrari',
          },
          {
            label: '7.Porshe',
          },
        ],
      },
      {
        label: 'Movie',
        children: [
          {
            label: '1.Ninja-Turtle',
          },
          {
            label: '2.Shang-Chi',
          },
          {
            label: '3.Spider-Man',
          },
          {
            label: '4.Bad-Man',
          },
          {
            label: '5.Avenger-Endgame',
          },
          {
            label: '6.Ant-Man',
          },
          {
            label: '7.Ninja-Go',
          },
        ],
      },
    ]);

    const unassignBox = ref<Tree[]>([]);
    const handleNodeClick = (data: Tree) => {
      selectedTree.value = data;
    };

    // ==========> Turn to Unassign Listbox <==========
    const doubleClickAssignNode = () => {
      doubleClickParentAssignNode();
      doubleClickSingleAssignNode();
    };

    const doubleClickParentAssignNode = () => {
      let target = JSON.parse(JSON.stringify(selectedTree.value));
      let items: Tree;
      if (!matchParentAssign(target.label)) {
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

    const doubleClickSingleAssignNode = () => {
      let target = JSON.parse(JSON.stringify(selectedTree.value));
      let items: Tree;

      data.value.filter((myData) => {
        myData.children?.filter((list) => {
          if (list.label === target.label) {
            items = JSON.parse(JSON.stringify(myData));
            items.children = [];
            items.children?.push(target);

            if (matchParentAssign(items.label)) {
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
            } else {
              let childData: Tree;
              unassignBox.value.filter((child) => {
                if (child.label === target.label) {
                  items = JSON.parse(JSON.stringify(child));
                  childData = items;

                  let indexOfParent: number;
                  for (let index = 0; index < unassignBox.value.length; index++) {
                    const element = unassignBox.value[index];
                    if (element.label == child.label) {
                      indexOfParent = index;
                      unassignBox.value.splice(indexOfParent, 1);
                    }
                  }
                }
              });

              data.value.filter((find) => {
                if (find.label === target.label) {
                  childData.children?.forEach((currentChilds) => {
                    find.children?.push(currentChilds);
                  });
                }
              });
            }
          }
        });
      });
    };

    const matchParentAssign = (parentName: string) => {
      const filterResult = unassignBox.value.filter((value) => value.label === parentName);
      if (filterResult.length !== 0) {
        return true;
      } else {
        return false;
      }
    };

    // ==========> Turn Back to Assign Listbox <==========
    const doubleClickUnssignNode = () => {
      doubleClickParentUnassignNode();
      doubleClickSingleUnassignNode();
    };
    const doubleClickParentUnassignNode = () => {
      let target = JSON.parse(JSON.stringify(selectedTree.value));
      let items: Tree;
      if (!matchParentUnassign(target.label)) {
        unassignBox.value.filter((myData) => {
          if (myData.label === target.label) {
            items = JSON.parse(JSON.stringify(myData));
            data.value.push(items);

            let indexOfParent: number;
            for (let index = 0; index < unassignBox.value.length; index++) {
              const element = unassignBox.value[index];
              if (element.label == myData.label) {
                indexOfParent = index;
                unassignBox.value.splice(indexOfParent, 1);
              }
            }
          }
        });
      } else {
        let childData: Tree;
        unassignBox.value.filter((child) => {
          if (child.label === target.label) {
            items = JSON.parse(JSON.stringify(child));
            childData = items;

            let indexOfParent: number;
            for (let index = 0; index < unassignBox.value.length; index++) {
              const element = unassignBox.value[index];
              if (element.label == child.label) {
                indexOfParent = index;
                unassignBox.value.splice(indexOfParent, 1);
              }
            }
          }
        });

        data.value.filter((find) => {
          if (find.label === target.label) {
            childData.children?.forEach((currentChilds) => {
              find.children?.push(currentChilds);
            });
          }
        });
      }
    };

    const doubleClickSingleUnassignNode = () => {
      let target = JSON.parse(JSON.stringify(selectedTree.value));
      let items: Tree;

      unassignBox.value.filter((myData) => {
        myData.children?.filter((list) => {
          if (list.label === target.label) {
            items = JSON.parse(JSON.stringify(myData));
            items.children = [];
            items.children?.push(target);

            if (matchParentUnassign(items.label)) {
              data.value.filter((match) => {
                if (match.label === items.label) {
                  match.children?.push(list);
                }
              });
            } else {
              data.value.push(items);
            }
            myData.children = myData.children?.filter((found) => {
              return found.label !== target.label;
            });

            if (myData.children?.length == 0) {
              let indexOfParent: number;
              for (let index = 0; index < unassignBox.value.length; index++) {
                const element = unassignBox.value[index];
                if (element.label == myData.label) {
                  indexOfParent = index;
                  unassignBox.value.splice(indexOfParent, 1);
                }
              }
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
          }
        });
      });
    };

    // ========= Button Assign transfer =========
    const btnTransferAllParentAssignNode = () => {
      data.value.filter((parentData) => {
        if (!matchParentAssign(parentData.label)) {
          unassignBox.value.push(parentData);
          data.value = [];
        } else {
          unassignBox.value.filter((list) => {
            if (list.label == parentData.label) {
              parentData.children?.filter((value) => {
                list.children?.push(value);
              });
            }
          });
        }
      });
    };

    const btnTransferSingleAssignNode = () => {
      let target = JSON.parse(JSON.stringify(selectedTree.value));
      let items: Tree;

      data.value.filter((myData) => {
        myData.children?.filter((list) => {
          if (list.label === target.label) {
            items = JSON.parse(JSON.stringify(myData));
            items.children = [];
            items.children?.push(target);

            if (matchParentAssign(items.label)) {
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
            } else {
              let childData: Tree;
              unassignBox.value.filter((child) => {
                if (child.label === target.label) {
                  items = JSON.parse(JSON.stringify(child));
                  childData = items;

                  let indexOfParent: number;
                  for (let index = 0; index < unassignBox.value.length; index++) {
                    const element = unassignBox.value[index];
                    if (element.label == child.label) {
                      indexOfParent = index;
                      unassignBox.value.splice(indexOfParent, 1);
                    }
                  }
                }
              });

              data.value.filter((find) => {
                if (find.label === target.label) {
                  childData.children?.forEach((currentChilds) => {
                    find.children?.push(currentChilds);
                  });
                }
              });
            }
          }
        });
      });
    };

    // ========== Call function select single node Assign =========
    const singleTransferAssignBtn = () => {
      if (selectedTree.value) {
        btnTransferSingleAssignNode();
        doubleClickParentAssignNode();
      }
    };

    // ========== Call function select single node Unassign =========
    const singleTransferUnassignBtn = () => {
      if (selectedTree.value) {
        btnTransferSingleUnassignNode();
        doubleClickParentUnassignNode();
      }
    };

    // ========== Button Unassign Transfer ==========
    const btnTransferAllParentUnAssignNode = () => {
      unassignBox.value.filter((parentData) => {
        if (!matchParentUnassign(parentData.label)) {
          data.value.push(parentData);
          unassignBox.value = [];
        } else {
          data.value.filter((list) => {
            if (list.label == parentData.label) {
              parentData.children?.filter((value) => {
                list.children?.push(value);
              });
            }
          });
        }
      });
    };

    const btnTransferSingleUnassignNode = () => {
      let target = JSON.parse(JSON.stringify(selectedTree.value));
      let items: Tree;

      unassignBox.value.filter((myData) => {
        myData.children?.filter((list) => {
          if (list.label === target.label) {
            items = JSON.parse(JSON.stringify(myData));
            items.children = [];
            items.children?.push(target);

            if (matchParentUnassign(items.label)) {
              data.value.filter((match) => {
                if (match.label === items.label) {
                  match.children?.push(list);
                }
              });
            } else {
              data.value.push(items);
            }
            myData.children = myData.children?.filter((found) => {
              return found.label !== target.label;
            });

            if (myData.children?.length == 0) {
              let indexOfParent: number;
              for (let index = 0; index < unassignBox.value.length; index++) {
                const element = unassignBox.value[index];
                if (element.label == myData.label) {
                  indexOfParent = index;
                  unassignBox.value.splice(indexOfParent, 1);
                }
              }
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
          }
        });
      });
    };

    const matchParentUnassign = (parentName: string) => {
      const filterResult = data.value.filter((value) => value.label === parentName);
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

      // element plus event target value
      handleNodeClick,

      // Return Back Unassignlist Box
      doubleClickAssignNode,
      doubleClickParentAssignNode,
      doubleClickSingleAssignNode,

      // Turn Back Assignlist Box
      doubleClickUnssignNode,
      doubleClickParentUnassignNode,
      doubleClickSingleUnassignNode,

      // Button Transfer
      btnTransferAllParentAssignNode,
      btnTransferSingleAssignNode,

      btnTransferAllParentUnAssignNode,
      btnTransferSingleUnassignNode,

      // this function call 2 option to join in this functions
      singleTransferUnassignBtn,
      singleTransferAssignBtn,
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
  display: block;
  empty-cells: show;
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
  display: block;
  position: relative;
  width: 500px;
  height: 350px;
  overflow-y: scroll;

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
.button-wraper {
  width: 100px;
  height: 200px;
}
.button-transfer button {
  display: inline-block;
  width: 100%;
  margin: 20px 0px 0px 0px;
}
.button {
  align-items: center;
  appearance: none;
  background-color: #fcfcfd;
  border-radius: 4px;
  border-width: 0;
  box-shadow: rgba(45, 35, 66, 0.4) 0 2px 4px, rgba(45, 35, 66, 0.3) 0 7px 13px -3px, #d6d6e7 0 -3px 0 inset;
  box-sizing: border-box;
  color: #36395a;
  cursor: pointer;
  display: inline-flex;
  font-family: 'JetBrains Mono', monospace;
  height: 30px;
  justify-content: center;
  line-height: 1;
  list-style: none;
  overflow: hidden;
  padding-left: 16px;
  padding-right: 16px;
  position: relative;
  text-align: left;
  text-decoration: none;
  transition: box-shadow 0.15s, transform 0.15s;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  white-space: nowrap;
  will-change: box-shadow, transform;
  font-size: 18px;
}

.button:focus {
  box-shadow: #d6d6e7 0 0 0 1.5px inset, rgba(45, 35, 66, 0.4) 0 2px 4px, rgba(45, 35, 66, 0.3) 0 7px 13px -3px,
    #d6d6e7 0 -3px 0 inset;
}

.button:hover {
  box-shadow: rgba(45, 35, 66, 0.4) 0 4px 8px, rgba(45, 35, 66, 0.3) 0 7px 13px -3px, #d6d6e7 0 -3px 0 inset;
  transform: translateY(-2px);
}

.button:active {
  box-shadow: #d6d6e7 0 3px 7px inset;
  transform: translateY(2px);
}
.button i {
  text-align: center;
  position: relative;
  left: 40%;
}
</style>
