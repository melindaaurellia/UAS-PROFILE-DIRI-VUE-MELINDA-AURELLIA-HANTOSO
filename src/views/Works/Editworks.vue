<template>
  <div class="card shadow mt-3">
    <div class="card-body">
      <h5 class="card-title">Edit</h5>
      <form class="row g-3" @submit.prevent="update">
        <div class="col-md-6">
          <label for="inputEmail4" class="form-label">Nama</label>
          <input
            type="text"
            class="form-control"
            id="inputEmail4"
            v-model="work.nama"
          />
          <div class="alert alert-danger" v-if="validation.nama">
            {{ validation.nama[0] }}
          </div>
        </div>
        <div class="col-md-6">
        <label for="inputEmail4" class="form-label">Tahun</label>
          <input
            type="text"
            class="form-control"
            id="inputEmail4"
            v-model="work.tahun"
          />
          <div class="alert alert-danger" v-if="validation.tahun">
            {{ validation.tahun[0] }}
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
    const work = reactive({
      nama: "",
      tahun: "",
    });

    let works = ref([]);
    const validation = ref([]);

    const router = useRouter();

    const route = useRoute();

    onMounted(() => {
      axios
        .get(` http://127.0.0.1:8000/api/works/${route.params.id}/edit`)
        .then((Response) => {
          console.log(Response.data.data.nama);

          work.nama = Response.data.data.nama;
          work.tahun = Response.data.data.tahun;
        })
        .catch((error) => {
          console.log(error.Response.data);
        });

        axios
      .get("http://127.0.0.1:8000/api/works")
      .then((response) => {
       works.value = response.data.data;
       console.log(response);
      })
      .catch(error => {
        console.log(error);
      });
    });

    function update() {
      let nama = work.nama;
      let tahun = work.tahun;
      

      axios
        .put(` http://127.0.0.1:8000/api/works/${route.params.id}`, {
          nama: nama,
          tahun: tahun,
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
      work,
      validation,
      router,
      update,
      route,
      works,
    };
  },
};
</script>