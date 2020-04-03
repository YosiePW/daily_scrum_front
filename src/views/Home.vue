<template>
    <div class="container-fluid page-body-wrapper">
      <div class="main-panel">
        <div class="content-wrapper">
          <div class="row">
            <div class="col-lg-12 grid-margin stretch-card">
              <div class="card">
                <div class="card-body">
                  <p class="card-title float-left"><i class="mdi mdi-format-list-bulleted menu-icon"></i></p>
                  <p class="card-description float-right">
                    <b-button variant="success" v-b-modal.modalDailyScrum v-on:click="Add"><i class="mdi mdi-plus btn-icon-prepend"></i>Add Activity</b-button>
                  </p>
                  <div class="table-responsive">
                      <b-table striped hover :items="dataDaily_scrum" :fields="fields">
                  <template v-slot:cell(daily)="data">
                    <b-badge variant="primry">{{ data.item.daily_scrums}}</b-badge>
                  </template>
                  <template v-slot:cell(Aksi)="data">
                    <b-button size="sm" variant="danger" v-on:click="Drop(data.item.id)"><i class="mdi mdi-delete btn-icon-prepend"></i></b-button>
                  </template>
                </b-table>
                  
                    <b-pagination
                  v-model="currentPage"
                  :per-page="perPage"
                  :total-rows="rows"
                  align="center"
                  v-on:input="pagination">
                </b-pagination>

                <b-toast id="loadingToast" title="Processing Data" no-auto-hide>
                  <b-spinner label="Spinning" variant="success"></b-spinner>
                  <strong class="text-success">Loading...</strong>
                </b-toast>

                <!-- toast untuk tampilan message box -->
                <b-toast id="message" title="Message">
                  <strong class="text-success">{{ message }}</strong>
                </b-toast>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

 <b-modal 
      id="modalDailyScrum"
      @ok="Save"
    >
      <template v-slot:modal-title>
        Form Daily Scrum
      </template>
        <form ref="form">
         <div class="form-group">
            <label for="team" class="col-form-label">Team</label>
            <select class="form-control" name="team" id="team" v-model="team">
              <option value="BEON" checked>BEON</option>
              <option value="DDS">DDS</option>
              <option value="DOT">DOT</option>
              <option value="node1">Node 1</option>
              <option value="node2">Node 2</option>
              <option value="react1">React 1</option>
              <option value="react2">React 2</option>
              <option value="laravel">Laravel</option>
              <option value="laravel_vue">Laravel & Vue</option>
              <option value="android">Android</option>
            </select>
          </div>
          <div class="form-group">
            <label for="activity_yesterday" class="col-form-label">Activity Yesterday</label>
            <input type="text" name="activity_yesterday" class="form-control" id="activity_yesterday" placeholder="Activity Yesterday" v-model="activity_yesterday">
          </div>
          <div class="form-group">
            <label for="activity_today" class="col-form-label">Activity Today</label>
            <input type="text" name="activity_today" class="form-control" id="activity_today" placeholder="Activity Today" v-model="activity_today">
          </div>
          <div class="form-group">
            <label for="problem_yesterday" class="col-form-label"> Problem Yesterday </label>
            <input type="text" name="problem_yesterday" class="form-control" id="problem_yesterday" placeholder="Problem Yesterday" v-model="problem_yesterday">
          </div>
          <div class="form-group">
            <label for="solution" class="col-form-label">Solution</label>
            <input type="text" name="solution" class="form-control" id="solution" placeholder="Solution" v-model="solution">
          </div>
        </form>
    </b-modal>
  </div>
</div>
</template>

<script>
module.exports = {
  data : function(){
    return {
      search: "",
      id: "",
      id_users: "this.id_users",
      team: "",
      activity_yesterday: "",
      activity_today: "",
      problem_yesterday: "",
      solution: "",
      tanggal:"",
      action: "",
      message: "",
      currentPage: 1,
      rows: 0,
      perPage: 10,
      key: "",
      dataDaily_scrum: [],
      daily_scrums: [],
      users: [],
      fields: ["tanggal", "activity_yesterday", "activity_today", "problem_yesterday","solution" ,"Aksi"],
    }
  },

 methods: {
    getData: function() {
        let conf = { headers: { Authorization: "Bearer " + this.key } };
        let offset = (this.currentPage - 1) * this.perPage;
        // let id_users = this.response.data.user.id;
        // this.$bvToast.show("loadingToast");
       this.axios
        .get("/login/check", conf)
        .then(response => {
          this.id_users = response.data.user.id;
          this.axios.get("/dailyscrum/" + this.perPage + "/" + offset + "/" + response.data.user.id, conf)
            .then(response => {
              this.status = response.data.status;
              if (response.data.status == 1) {
                this.dataDaily_scrum = response.data.daily_scrums;
                console.log(response.data.count);
     
                if (response.data.count == 0) {
                  this.status = 0;
          
                }
              } else {
                console.log("Data Tidak Ditemukan");
          
              }
            })
            // .catch(error => {
            //   console.log(error);
            //   this.loading = false;
            // });
        })
        // .catch(error => {
        //   console.log(error);
        //   this.loading = false;
        // });
      },

    pagination : function(){
      if(this.search == ""){
        this.getData();
      } else {
        this.searching();
      }
    },

    Add : function(){
      this.action = "insert";
      this.team = "";
      this.activity_yesterday = "";
      this.activity_today = ""; 
      this.problem_yesterday = ""; 
      this.solution = ""; 
      
    },

    Save : function(){
      let conf = { headers: { "Authorization" : 'Bearer ' + this.key } };
      this.$bvToast.show("loadingToast");
      if(this.action === "insert"){
        let form = new FormData();
        form.append("id", this.id);
        form.append("team", this.team);
        form.append("activity_yesterday", this.activity_yesterday);
        form.append("activity_today", this.activity_today);
        form.append("problem_yesterday", this.problem_yesterday);
        form.append("solution", this.solution);


        this.axios.post("/daily", form, conf)
        .then(response => {
          this.$bvToast.hide("loadingToast");
          if(this.search == ""){
            this.getData();
          } else {
            this.searching();
          }
          this.message = response.data.message;
          this.$bvToast.show("message");
        })
        .catch(error => {
          console.log(error);
        });
      } else {
        let form = {
          team: this.team,
          activity_yesterday: this.activity_yesterday,
          activity_today: this.activity_today,
          problem_yesterday: this.problem_yesterday,
          solution: this.solution
        }
        this.axios.put("/daily/" + this.id, form, conf)
        .then(response => {
          this.$bvToast.hide("loadingToast");
          if(this.search == ""){
            this.getData();
          } else {
            this.searching();
          }
          this.message = response.data.message;
          this.$bvToast.show("message");
        })
        .catch(error => {
          console.log(error);
        });
      }
    },

    Drop : function(id){
      let conf = { headers: { "Authorization" : "Bearer " + this.key} };
      if(confirm('Apakah anda yakin ingin menghapus data ini?')){
        this.$bvToast.show("loadingToast");
        this.axios.delete("/daily/" + id, conf)
        .then(response => {
            this.getData();
            this.$bvToast.hide("loadingToast");
            this.message = response.data.message;
            this.$bvToast.show("message");
        })
        .catch(error => {
          console.log(error);
        });
      }
    },
  },
  mounted(){
    this.key = localStorage.getItem("Authorization");
    this.getData();
  }
}
</script>
