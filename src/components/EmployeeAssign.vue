<template>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">

    <div>
        <header>
            <HeaderComponent />
        </header>

        <main>
            <BodyComponent />
            <div class="LinkedVehicleAssignment">
                <!-- Button to toggle visibility -->
                <button type="button" class="btn btn-primary" @click="listOfCars">List of all cars</button>

                <ol></ol>
                <button type="button" class="btn btn-primary" @click="toggleAssignVehicle">Create new car</button>

            </div>

            <!-- Your additional content here -->
            <div class="AssignVehicle" v-if="showAssignVehicle">
                <div>
                    <!-- Input field with dropdown for Manufacturer -->
                    <div class="input-field">
                        <label for="manufacturer">Manufacturer:</label>
                        <ol></ol>
                        <div class="dropdown">
                            <input type="text" id="manufacturer" v-model="selectedManufacturer" @input="filterManufacturers" placeholder="Select or type">
                            <div class="dropdown-content">
                                <div v-for="manufacturer in filteredManufacturers" :key="manufacturer.manufacturerID" @click="selectManufacturer(manufacturer)">
                                    {{ manufacturer.brandName }}
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Input field with dropdown for Model -->
                    <div class="input-field">
                        <label for="model">Model:</label>
                        <div class="dropdown">
                            <input type="text" id="model" v-model="selectedModel" @input="filterModels" placeholder="Select or type">
                            <div class="dropdown-content">
                                <div v-for="model in filteredModels" :key="model.carModelID" @click="selectModel(model)">
                                    {{ model.model }}
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Input field with dropdown for Employee -->
                    <div class="input-field">
                        <label for="employee">Employee:</label>
                        <div class="dropdown">
                            <input type="text" id="employee" v-model="selectedEmployee" @input="filterEmployees" placeholder="Select or type">
                            <div class="dropdown-content">
                                <div v-for="employee in filteredEmployees" :key="employee.employeeID" @click="selectEmployee(employee)">
                                    {{ employee.username }}
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Input field with dropdown for Customer -->
                    <div class="input-field">
                        <label for="customer">Customer:</label>
                        <div class="dropdown">
                            <input type="text" id="customer" v-model="selectedCustomer" @input="filterCustomers" placeholder="Select or type">
                            <div class="dropdown-content">
                                <div v-for="customer in filteredCustomers" :key="customer.customerID" @click="selectCustomer(customer)">
                                    {{ customer.username }}
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Input field for Registration Plate -->
                    <div class="input-field">
                        <label for="input4">Registration Plate:</label>
                        <input type="text" id="registrationPlate" v-model="registrationPlate" placeholder="Enter valid Bulgarian registration plate">
                    </div>

                    <!-- Input field for VIN -->
                    <div class="input-field">
                        <label for="input4">VIN:</label>
                        <input type="text" id="vin" v-model="vin" placeholder="Enter car VIN number">
                    </div>

                    <!-- Button to assign vehicle -->
                    <button type="button" class="btn btn-success" @click="assignVehicle">Assign</button>
                </div>

                <!-- Services section -->
                <div class="services">
                    <div class="input-field">
                        <label for="service">Services:</label>
                        <ol></ol>
                        <div class="dropdown">
                            <input type="text" id="service" v-model="selectedService" @input="filterServices" placeholder="Select or type">
                            <div class="dropdown-content">
                                <div v-for="service in filteredServices" :key="service.serviceID" @click="addServiceToTable(service)">
                                    {{ service.name }}
                                </div>
                            </div>
                        </div>
                        <table class="table table-hover">
                            <thead>
                                <tr>
                                    <th scope="col">#</th>
                                    <th scope="col">Service</th>
                                    <th scope="col">Employee</th>
                                    <th scope="col">Price</th>
                                    <th scope="col">Action</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(service, index) in selectedServices" :key="service.serviceID">
                                    <th scope="row">{{ index + 1 }}</th>
                                    <td>{{ service.name }}</td>
                                    <td>{{ service.employee }}</td>
                                    <td>{{ service.price }}</td>
                                    <td>
                                        <button type="button" class="btn btn-primary" @click="deleteService(index)">Delete</button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- List of Cars section -->
            <div class="ListOfCars" v-if="carList">
                <div class="Vehicles">
                    <div class="input-field">
                        <label for="model">Vehicles:</label>
                        <ol></ol>
                        <div class="dropdown">
                            <input type="text" id="vehicleModel" v-model="selectedVehicleModel" @input="filterVehicleModels" placeholder="Select or search">
                            <div class="dropdown-content">
                                <div v-for="model in filteredVehicleModels" :key="model.id" @click="selectVehicleModel(model)">
                                    {{ model.name }}
                                </div>
                            </div>
                        </div>
                        <!-- Table for displaying vehicles -->
                        <table class="table table-hover">
                            <thead>
                                <tr>
                                    <th scope="col">#</th>
                                    <th scope="col">Car</th>
                                    <th scope="col">Customer</th>
                                    <th scope="col">Progress</th>
                                    <th scope="col">Employee</th>
                                    <th scope="col">Price</th>
                                    <th scope="col">Edit</th>
                                    <th scope="col">Delete</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(vehicle, index) in filteredVehicles" :key="vehicle.id">
                                    <th scope="row">{{ index + 1 }}</th>
                                    <td>{{ vehicle.name }}</td>
                                    <td>{{ vehicle.customer }}</td>
                                    <td>{{ vehicle.progress }}</td>
                                    <td>{{ vehicle.employee }}</td>
                                    <td>{{ vehicle.price }}</td>
                                    <td>
                                        <button type="button" class="btn btn-primary" @click="editVehicle(vehicle)">Edit</button>
                                    </td>
                                    <td>
                                        <button type="button" class="btn btn-primary" @click="deleteVehicle(vehicle)">Delete</button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                    <!-- Pagination -->
                    <nav aria-label="Page navigation example">
                        <ul class="pagination">
                            <li class="page-item"><a class="page-link" href="#">Previous</a></li>
                            <li class="page-item"><a class="page-link" href="#">1</a></li>
                            <li class="page-item"><a class="page-link" href="#">2</a></li>
                            <li class="page-item"><a class="page-link" href="#">3</a></li>
                            <li class="page-item"><a class="page-link" href="#">Next</a></li>
                        </ul>
                    </nav>
                </div>
            </div>
            <!-- End of List of Cars section -->
        </main>

        <footer>
            <FooterComponent />
        </footer>
    </div>
</template>

<script>
    import HeaderComponent from "@/components/HeaderComponent.vue";
    import BodyComponent from "@/components/BodyComponent.vue";
    import FooterComponent from "@/components/FooterComponent.vue";

    export default {
        components: {
            HeaderComponent,
            BodyComponent,
            FooterComponent
        },
        data() {
            return {
                selectedManufacturer: "",
                selectedModel: "",
                manufacturers: [],
                models: [],
                showAssignVehicle: false,
                carList: true,
                selectedEmployee: "",
                employees: [],
                selectedCustomer: "",
                customers: [],
                selectedService: "",
                services: [],
                selectedServices: [], // Array to store selected services
                vin: "",
                registrationPlate: ""
            };
        },
        computed: {
            // Computed properties for filtering dropdown options
            filteredManufacturers() {
                return this.manufacturers.filter(manufacturer =>
                    manufacturer.brandName.toLowerCase().includes(this.selectedManufacturer.toLowerCase())
                );
            },
            filteredModels() {
                if (!this.selectedManufacturer) {
                    return [];
                }
                const selectedManufacturer = this.manufacturers.find(manufacturer => manufacturer.brandName === this.selectedManufacturer);
                if (!selectedManufacturer) {
                    return [];
                }
                return selectedManufacturer.carModels;
            },
            filteredEmployees() {
                return this.employees.filter(employee =>
                    employee.username.toLowerCase().includes(this.selectedEmployee.toLowerCase())
                );
            },
            filteredCustomers() {
                return this.customers.filter(customer =>
                    customer.username.toLowerCase().includes(this.selectedCustomer.toLowerCase())
                );
            },
            filteredServices() {
                return this.services.filter(service =>
                    service.name.toLowerCase().includes(this.selectedService.toLowerCase())
                );
            }
        },
        methods: {
            async fetchManufacturers() {
                try {
                    const response = await fetch("https://backend.smartgarageproject.com/api/Manufacturer");
                    const data = await response.json();
                    this.manufacturers = data;
                } catch (error) {
                    console.error("Error fetching manufacturers:", error);
                }
            },
            async fetchEmployees() {
                try {
                    const response = await fetch("https://backend.smartgarageproject.com/api/Employee");
                    const data = await response.json();
                    this.employees = data;
                } catch (error) {
                    console.error("Error fetching employees:", error);
                }
            },
            async fetchCustomers() {
                try {
                    const response = await fetch("https://backend.smartgarageproject.com/api/customers");
                    const data = await response.json();
                    this.customers = data;
                } catch (error) {
                    console.error("Error fetching customers:", error);
                }
            },
            async fetchServices() {
                try {
                    const response = await fetch("https://backend.smartgarageproject.com/api/Service");
                    const data = await response.json();
                    this.services = data;
                } catch (error) {
                    console.error("Error fetching services:", error);
                }
            },
            async assignVehicle() {
                const requestBody = this.generateRequestBody();
                try {
                    const response = await fetch("https://backend.smartgarageproject.com/api/LinkedVehicles", {
                        method: "POST",
                        headers: {
                            "Content-Type": "application/json"
                        },
                        body: JSON.stringify(requestBody)
                    });
                    const data = await response.json();
                    console.log(data); // Handle response as needed
                } catch (error) {
                    console.error("Error assigning vehicle:", error);
                }
            },
            toggleAssignVehicle() {
                this.showAssignVehicle = !this.showAssignVehicle;
                if (this.showAssignVehicle && this.carList) {
                    this.carList = !this.carList;
                }
            },
            generateRequestBody() {
                return {
                    VIN: this.vin,
                    modelid: this.selectedModel,
                    ManufacturerID: this.selectedManufacturer,
                    customerid: this.selectedCustomer,
                    employeeid: this.selectedEmployee,
                    LicensePlate: this.registrationPlate,
                    YearOfCreation: 2023,
                    InvoiceID: 1,
                    LinkedVehicleServices: this.selectedServices.map(service => ({
                        serviceName: service.name,
                        "ServiceID": 1

                    }))
                };
            },
            listOfCars() {
                this.carList = !this.carList;
                if (this.carList && this.showAssignVehicle) {
                    this.showAssignVehicle = !this.showAssignVehicle;
                }
            },
            filterManufacturers() {
                this.$nextTick(() => {
                    document.querySelector("#manufacturer .dropdown-content").style.display = "block";
                });
            },
            filterModels() {
                this.$nextTick(() => {
                    document.querySelector("#model .dropdown-content").style.display = "block";
                });
            },
            selectManufacturer(manufacturer) {
                this.selectedManufacturer = manufacturer.brandName;
                this.models = manufacturer.carModels;
                document.querySelector("#manufacturer .dropdown-content").style.display = "none";
            },
            selectModel(model) {
                this.selectedModel = model.model;
                document.querySelector("#model .dropdown-content").style.display = "none";
            },
            selectEmployee(employee) {
                this.selectedEmployee = employee.username;
                document.querySelector("#employee .dropdown-content").style.display = "none";
            },
            filterEmployees() {
                this.$nextTick(() => {
                    document.querySelector("#employee .dropdown-content").style.display = "block";
                });
            },
            filterCustomers() {
                this.$nextTick(() => {
                    document.querySelector("#customer .dropdown-content").style.display = "block";
                });
            },
            selectCustomer(customer) {
                this.selectedCustomer = customer.username;
                document.querySelector("#customer .dropdown-content").style.display = "none";
            },
            selectService(service) {
                this.selectedService = service.name;
                document.querySelector("#service .dropdown-content").style.display = "none";
            },
            filterServices() {
                this.$nextTick(() => {
                    document.querySelector("#service .dropdown-content").style.display = "block";
                });
            },
            // Method to add selected service to the table
            addServiceToTable(service) {
                this.selectedServices.push({
                    id: service.serviceID,
                    name: service.name,
                    employee: service.employee,
                    price: service.price
                });
            },

            // Method to delete service from the table
            deleteService(index) {
                this.selectedServices.splice(index, 1);
            },
            // Other methods for editing and deleting vehicles
            editVehicle(vehicle) {
                // Implement logic for editing vehicles
            },
            deleteVehicle(vehicle) {
                // Implement logic for deleting vehicles
            }
        },
        mounted() {
            this.fetchManufacturers();
            this.fetchEmployees();
            this.fetchCustomers();
            this.fetchServices();
        }
    };
</script>

<style scoped>
    .AssignVehicle {
        left: 23.5%;
        top: 12%;
        position: absolute;
    }

    .services {
        left: 95%;
        top: 0;
        position: absolute;
    }

    .ListOfCars {
        left: 23.5%;
        top: 12%;
        position: absolute;
    }

    .LinkedVehicleAssignment {
        left: 5%;
        top: 15%;
        position: absolute;
    }

    .services {
        position: absolute;
        right: 20vh;
        width: 600px;
        top: 12%;
    }

    label {
        font-weight: bold;
    }

    input {
        width: calc(100% - 40px); /* Adjust the width of the input field */
        padding: 8px;
        border: 1px solid #ccc;
        border-radius: 4px;
        box-sizing: border-box;
        color: black;
    }

    .dropdown {
        position: relative;
        display: inline-block;
    }

    .dropdown-content {
        display: none;
        position: absolute;
        background-color: #f9f9f9;
        color: black;
        min-width: 100%;
        z-index: 1;
        border: 1px solid #ddd;
        border-radius: 4px;
        overflow-y: auto;
        max-height: 150px; /* Adjust the max height of the dropdown menu */
    }

        .dropdown-content div {
            padding: 8px;
            cursor: pointer;
            color: black; /* Set the text color to black */
            font-size: 14px; /* Set the font size to 14px */
        }

    .dropdown:hover .dropdown-content {
        display: block;
    }
</style>
