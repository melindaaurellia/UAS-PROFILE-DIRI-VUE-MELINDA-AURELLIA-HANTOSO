<template>
  <div class="card shadow mt-3">
    <div class="card-body">
      <h5 class="card-title">Edit profile</h5>
      <form class="row g-3" @submit.prevent="update">
    
           <div class="col-md-6">
          <label for="inputEmail4" class="form-label">Email</label>
          <input
            type="text"
            class="form-control"
            id="inputEmail4"
            v-model="profile.email"
          />
          <div class="alert alert-danger" v-if="validation.email">
            {{ validation.email[0] }}
          </div>
        </div>

        <div class="col-md-6">
          <label for="inputEmail4" class="form-label">No Telepon</label>
          <input
            type="text"
            class="form-control"
            id="inputEmail4"
            v-model="profile.no_tlp"
          />
          <div class="alert alert-danger" v-if="validation.no_tlp">
            {{ validation.no_tlp[0] }}
          </div>
        </div>


        <div class="col-12">
          <button type="submit" class="btn btn-success">Edit</button>
        </div>
      </form>
    </div>
  </div>
</template>
<script>
import { onMounted, reactive, ref } from "vue";
import { useRoute, useRouter } from "vue-router";
import axios from "axios";
export default {
  setup() {
    const profile = reactive({
      nama: "",
      tgl: "",
    
    });

    let profiles = ref([]);
    const validation = ref([]);

    const router = useRouter();

    const route = useRoute();

    onMounted(() => {
      axios
        .get(` http://127.0.0.1:8000/api/profiles/${route.params.id}/edit`)
        .then((Response) => {
          console.log(Response.data.data.nama);

          profile.nama = Response.data.data.nama;
          profile.tgl = Response.data.data.tgl;
        
        })
        .catch((error) => {
          console.log(error.Response.data);
        });

        axios
      .get("http://127.0.0.1:8000/api/profiles")
      .then((response) => {
       profiles.value = response.data.data;
       console.log(response);
      })
      .catch(error => {
        console.log(error);
      });
    });

    function update() {
      let nama = profile.nama;
      let tgl = profile.tgl;
      

      axios
        .put(` http://127.0.0.1:8000/api/profiles/${route.params.id}`, {
          nama: nama,
          tgl: tgl,
         
        })
        .then(() => {
          router.push({
            name: "Home",
          });
        })
        .catch((error) => {
          console.log(error);
        });
    }
    return {
      profile,
      validation,
      router,
      update,
      route,
      profiles,
    };
  },
};
</script>