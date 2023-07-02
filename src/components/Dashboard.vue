<template>
  <div id="burgerTable">
    <div class="tableContainer">
      <legend class="tableLegend">Manage your Orders</legend>
      <table>
        <thead id="burgerTableHeading">
          <tr>
            <th class="orderId">#</th>
            <th>Client</th>
            <th>Bread</th>
            <th>Beef</th>
            <th>Optionals</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody id="burgerTableRow">
          <tr v-for="burger in burgers" :key="burger.id">
            <td class="orderNumber">{{ burger.id }}</td>
            <td>{{ burger.name }}</td>
            <td>{{ burger.bread }}</td>
            <td>{{ burger.beef }}</td>
            <td>
              <ul>
                <li v-for="(optional, i) in burger.optionals" :key="i">
                  {{ optional }}
                </li>
              </ul>
            </td>
            <td class="actionColumn">
              <div class="statusContainer">
                <select name="status" class="status" @change="updateBurger($event, burger.id)">
                  <option value="">Select burger status</option>
                  <option v-for="s in status" :key="s.id" :value="s.tipo" :selected="burger.status === s.tipo">
                    {{ s.tipo }}
                  </option>
                </select>
                <button class="deleteBtn" @click="deleteBurger(burger.id)">Cancel</button>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "Dashboard",
  data() {
    return {
      burgers: null,
      burgerId: null,
      status: [],
    };
  },
  methods: {
    async getOrders() {
      const req = await fetch("http://localhost:3000/burgers");
      const data = await req.json();

      this.burgers = data;

      console.log(this.burgers);

      this.getStatus()
    },
    async getStatus() {
      const req = await fetch('http://localhost:3000/status');
      const data = await req.json();

      this.status = data;
    },
    async deleteBurger(id) {
      const req = await fetch(`http://localhost:3000/burgers/${id}`, {
        method: "DELETE",
        headers: { "Content-Type" : "application/json" }
      });

      try {
          if (!req.ok) {
            console.error('Error deleting burger')
          }

          const res = await req.json();

          this.getOrders()
      } catch (error) {
        console.error('Error:', error);
      }
    },
    async updateBurger(ev, id) {
      const option = ev.target.value;

      const dataJson = JSON.stringify({ status: option });

      const req = await fetch(`http://localhost:3000/burgers/${id}`, {
        method: "PATCH",
        headers: { "Content-Type" : "application/json" },
        body: dataJson
      })

      const res = await req.json();
      console.log(res);
    }
  },
  mounted() {
    this.getOrders();
  },
};
</script>

<style scoped>
#burgerTable {
  max-width: 100%;
  margin: 20px auto;
}

.tableContainer {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
  border: 1px solid #ccc;
  table-layout: fixed;
}

.tableLegend {
  background-color: #222222;
  color: #fcba03;
  padding: 10px;
  text-align: center;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
}

#burgerTableHeading {
  background-color: #f2f2f2;
}

#burgerTableHeading th {
  padding: 15px;
  text-align: left;
  font-weight: bold;
}

#burgerTableRow td {
  padding: 15px;
  vertical-align: middle;
}

.orderId {
  width: 50px;
}

.statusContainer {
  display: flex;
  align-items: center;
}

.status {
  padding: 8px 12px;
  margin-right: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  flex: 1;
  max-width: 150px;
}

.deleteBtn {
  padding: 8px 12px;
  background-color: #222;
  color: #fcba03;
  border: 2px solid #222;
  border-radius: 5px;
  cursor: pointer;
  transition: 0.5s;
  flex: 1;
  max-width: 150px;
}

.deleteBtn:hover {
  background-color: transparent;
  color: #222;
}

ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

ul li {
  margin-bottom: 10px;
}

ul li:last-child {
  margin-bottom: 0;
}

/* Responsiveness based on device breakpoints */

@media screen and (max-width: 576px) {
  /* Styles for mobile devices */
  .statusContainer {
    flex-direction: column;
  }

  .status,
  .deleteBtn {
    width: 100%;
    margin-top: 10px;
  }
}

@media screen and (min-width: 577px) and (max-width: 768px) {
  /* Styles for tablets */
  .tableContainer {
    overflow-x: auto;
  }

  .statusContainer {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }

  .status,
  .deleteBtn {
    margin-bottom: 10px;
  }
}

@media screen and (min-width: 769px) and (max-width: 992px) {
  /* Styles for small desktops */
  .tableContainer {
    overflow-x: auto;
  }

  .statusContainer {
    display: flex;
    align-items: center;
  }

  .status {
    margin-right: 10px;
  }

  .deleteBtn {
    margin-left: auto;
  }
}

@media screen and (min-width: 993px) and (max-width: 1499px) {
  /* Styles for medium-sized desktops */
  .tableContainer {
    overflow-x: auto;
  }

  .statusContainer {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
  }

  .status {
    margin-right: 10px;
  }

  .deleteBtn {
    margin-top: 10px;
    margin-left: auto;
  }
}

@media screen and (min-width: 1500px) {
  /* Styles for large desktops and above */
  .tableContainer {
    overflow-x: hidden;
  }

  .statusContainer {
    display: flex;
    align-items: center;
  }

  .status {
    margin-right: 10px;
  }

  .deleteBtn {
    margin-left: 10px;
  }
}
</style>
