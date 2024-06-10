<template>
  <v-container>
    <v-row class="justify-end">
      <v-col cols="12">
        <v-card>
          <v-card>
            <v-card-text class="d-flex">
              <v-card-text class="d-flex justify-start align-center">
                <v-icon large class="mr-3">mdi-domain</v-icon>
                Empresas
              </v-card-text>
              <v-card-text class="d-flex justify-end align-center">
                <v-text-field label="Consultar Empresa" v-model="cnpj" append-icon="mdi-magnify" solo
                  @click:append="searchCompany"></v-text-field>
              </v-card-text>
            </v-card-text>
          </v-card>
          <v-divider></v-divider>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
  <v-container>
    <v-row>
      <v-col cols="12">
        <v-data-table :headers="headers" :items="items" :items-per-page="6" class="elevation-1" :fixed-header="true"
          :height="tableHeight" :item-key="itemKey">
          <template v-slot:item="{ item }">
            <tr>
              <td>{{ item.result.nome }}</td>
              <td>{{ item.cnpj }}</td>
              <td>{{ item.result.situacao }}</td>
            </tr>
          </template>
        </v-data-table>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import type { EmpresaParsed } from '../interfaces/CompanyData.ts';
import { useFetchCompany } from '../composables/useFetchCompany';

const { getAllCompany, consultCompany } = useFetchCompany();

const cnpj = ref('');
const headers = ref([
  { text: 'Nome', value: 'result.nome' },
  { text: 'CNPJ', value: 'cnpj' },
  { text: 'Situação', value: 'result.situacao' },
]);
const items = ref<EmpresaParsed[]>([]);
const itemKey = ref('cnpj');

const searchCompany = async () => {
  await consultCompany({ cnpj: cnpj.value });
  await loadAllCompanies();
};

const loadAllCompanies = async () => {
  const response = await getAllCompany();
  if (Array.isArray(response)) {
    items.value = response;
  }
};
</script>

<style scoped></style>
