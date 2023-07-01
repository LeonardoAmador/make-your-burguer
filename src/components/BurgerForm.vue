<template>
    <div>
        <div id="envolveAllForm">
            <form id="burgerForm" @submit="createBurger">
                <div class="inputContainer">
                    <label for="name">Client Name: </label>
                    <input type="text" 
                            name="name" 
                            id="name" 
                            v-model="name" 
                            placeholder="Enter your name: ">
                </div>
                <div class="inputContainer">
                    <label for="bread">Choose the bread: </label>
                    <select name="bread" id="bread" v-model="bread">
                        <option v-for="bread in breads" :key="bread.id" :value="bread.tipo">
                            {{ bread.tipo }}
                        </option>
                    </select>
                </div>
                <div class="inputContainer">
                    <label for="beef">Choose the beef: </label>
                    <select name="beef" id="beef" v-model="beef">
                        <option v-for="beef in beefs" :key="beef.id" :value="beef.tipo">
                            {{ beef.tipo }}
                        </option>
                    </select>
                </div>
                <div class="inputContainer" id="optionalContainer">
                    <label id="optionalTitle" for="optional">Choose the optional: </label>
                    <div class="checkboxContainer" v-for="optional in optionalData" :key="optional.id">
                        <input type="checkbox" name="optionals" v-model="optionals" :value="optional.tipo">
                        <span>{{ optional.tipo }}</span>
                    </div>
                </div>
                <div class="inputContainer">
                    <input type="submit" class="submitBtn" value="Create Burger">
                </div>
            </form>
        </div>
    </div>
</template>

<script>
    export default {
        name: "BurgerForm",
        data() {
            return {
                breads: null,
                beefs: null,
                optionalData: null,
                name: null,
                bread: null,
                beef: null,
                optionals: [],
                status: "",
                msg: null
            }
        },
        methods: {
            async getIngredients() {
                const req = await fetch('http://localhost:3000/ingredientes');
                
                try {
                    if (!req.ok) {
                        throw new Error('Unavailable path.')
                    }
                    
                    const data = await req.json();
                    
                    this.breads = data.paes;
                    this.beefs = data.carnes;
                    this.optionalData = data.opcionais;

                } catch (error) {
                    console.error(error);
                }
            },
            async createBurger(e) {
                e.preventDefault();

                const data = {
                    name: this.name,
                    beef: this.beef,
                    bread: this.bread,
                    optionals: Array.from(this.optionals),
                    status: "Solicitado"
                }

                const req = await fetch('http://localhost:3000/burgers', {
                    method: "POST",
                    headers: { "Content-Type": "application/json" },
                    body: JSON.stringify(data)
                });

                const res = await req.json();

                this.cleanInputs();
            },
            cleanInputs() {
                this.name = "";
                this.beef = "";
                this.bread = "";
                this.optionals = "";
            },
        },
        mounted() {
            this.getIngredients();
        }
    }
</script>

<style scoped>
    #envolveAllForm {
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    #burgerForm {
        max-width: 400px;
        margin: 0 auto;
    }

    .inputContainer {
        display: flex;
        flex-direction: column;
        margin-bottom: 20px;
    }

    label {
        font-weight: bold;
        margin-bottom: 15px;
        color: #222;
        padding: 5px 10px;
        border-left: 4px solid #fcba03;
    }

    input, select {
        padding: 5px 10px;
        width: 300px;
    }

    #optionalContainer {
        flex-direction: row;
        flex-wrap: wrap;
    }

    #optionalTitle {
        width: 100%;
    }

    #checkboxContainer {
        display: flex;
        align-items: flex-start;
        width: 50%;
        margin-bottom: 20px;
    }

    .checkboxContainer span, 
    .checkboxContainer input {
        width: auto;
    }

    .checkboxContainer span {
        margin: 0px 20px;
        font-weight: bold;
    }

    .submitBtn {
        background-color: #222;
        color: #fcba03;
        font-weight: bold;
        border: 2px solid #222;
        padding: 10px;
        font-size: 16px;
        margin: 0 auto;
        cursor: pointer;
        transition: .5s;
    }

    .submitBtn:hover {
        background-color: transparent;
        color: #222;
    }
</style>