<template>
  <el-tabs v-model="activeName" class="h-screen flex flex-col items-center">
    <!-- SGPA Tab -->
    <!-- Formula: SGPA (Si) = ∑(Ci x Gi) / ∑Ci ,where Ci is the number of credits of the ith
    course and Gi is the grade point scored by the student in the ith course. -->

    <el-tab-pane
      label="SGPA"
      name="sgpa"
      class="relative flex flex-col items-center h-screen w-screen"
    >
      <h2 class="mb-2 mt-2 flex items-center">SGPA Calculator</h2>
      <el-card
        class="box-card mx-auto w-9/12 h-auto overflow-auto container mt-4"
      >
        <el-table :data="tableData" stripe style="width: 100%">
          <el-table-column prop="subject" label="Course Code/Name" width="auto">
            <template v-slot="scope">
              <el-input
                placeholder="Enter Course Code/Name"
                v-model="scope.row['subject']"
                autocomplete="off"
              ></el-input>
            </template>
          </el-table-column>
          <el-table-column prop="sem" label="Sem" width="auto">
            <template v-slot="scope">
              <el-select
                placeholder="Enter Sem"
                v-model="scope.row['sem']"
                autocomplete="off"
              >
                <el-option
                  v-for="sem in semisters"
                  :key="sem.value"
                  :label="sem.value"
                  :value="sem.value"
                >
                </el-option
              ></el-select>
            </template>
          </el-table-column>
          <el-table-column prop="credits" label="Subject Credits" width="auto">
            <template v-slot="scope">
              <el-input
                placeholder="Enter Subject Credits"
                v-model="scope.row['credits']"
                autocomplete="off"
              ></el-input>
            </template>
          </el-table-column>
          <el-table-column prop="grade" label="Subject Grade" width="auto">
            <template v-slot="scope">
              <el-select
                placeholder="Enter Subject Grade"
                v-model="scope.row['grade']"
                autocomplete="off"
              >
                <el-option
                  v-for="grade in grades"
                  :key="grade.value"
                  :label="grade.label"
                  :value="grade.value"
                >
                </el-option
              ></el-select>
            </template>
          </el-table-column>
          <el-table-column width="80">
            <template v-slot="scope">
              <el-button
                @click.prevent="deleteRow(scope.$index, scope.row)"
                type="danger"
                icon="el-icon-delete"
                circle
              >
              </el-button>
            </template>
          </el-table-column>
        </el-table>
        <el-divider></el-divider>
        <el-button
          class="w-full p-2"
          type="primary"
          plain
          @click="addRow"
          icon="el-icon-circle-plus-outline"
          >Add Row</el-button
        >
        <div class="flex w-full justify-end items-end">
          <el-button class="my-7 flex" type="primary" @click="submit"
            ><span class="tracking-widest">Calculate</span></el-button
          >
        </div>
        <span class="text-sm italic text-red-500 m-2"
          >* Please don't add W grade subjects</span
        >
      </el-card>

      <div class="flex box-card mx-auto w-9/12 container mt-2 mb-24">
        <el-card class="w-1/2 flex items-center justify-center mr-1 p-4">
          <h4 class="" style="color: #909399">
            Total Credits:
            <span class="text-2xl ml-2" style="color: #409eff">{{
              totalCredits
            }}</span>
          </h4>
        </el-card>
        <el-card class="w-1/2 flex items-center justify-center ml-1">
          <h4 style="color: #909399">
            SGPA:
            <span class="text-2xl ml-2" style="color: #409eff">{{ sgpa }}</span>
          </h4>
        </el-card>
      </div>
      <div class="flex absolute bottom-16 right-4">
        Developed By
        <a
          href="https://varuntv.netlify.app/"
          target="blank"
          style="text-decoration: none; color: #409eff"
          class="flex flex-col items-center justify-center ml-2"
          >Varun TV</a
        >
      </div>
    </el-tab-pane>

    <!-- CGPA Tab -->
    <!-- Formula: CGPA = ∑(Ci x Si) / ∑ Ci
where Si is the SGPA of the ith semester and Ci is the total number of credits in that semester. -->

    <el-tab-pane
      label="CGPA"
      name="cgpa"
      class="flex flex-col items-center h-screen w-screen"
    >
      <h2 class="mb-2 mt-2 flex items-center">CGPA Calculator</h2>
      <el-card
        class="box-card mx-auto w-9/12 h-auto overflow-auto container mt-4"
      >
        <el-table :data="cgpaTableData" stripe style="width: 100%">
          <el-table-column prop="sem" label="Sem" width="auto">
            <template v-slot="scope">
              <el-select
                placeholder="Enter Sem"
                v-model="scope.row['sem']"
                autocomplete="off"
              >
                <el-option
                  v-for="sem in semisters"
                  :key="sem.value"
                  :label="sem.value"
                  :value="sem.value"
                >
                </el-option
              ></el-select>
            </template>
          </el-table-column>
          <el-table-column
            prop="credits"
            label="Total Sem Credits"
            width="auto"
          >
            <template v-slot="scope">
              <el-input
                placeholder="Enter Total Sem Credits"
                v-model="scope.row['totalSemCredits']"
                autocomplete="off"
              ></el-input>
            </template>
          </el-table-column>
          <el-table-column prop="sgpa" label="SGPA" width="auto">
            <template v-slot="scope">
              <el-input
                placeholder="Enter SGPA"
                v-model="scope.row['sgpa']"
                autocomplete="off"
              ></el-input>
            </template>
          </el-table-column>
          <el-table-column width="80">
            <template v-slot="scope">
              <el-button
                @click.prevent="deleteCgpaRow(scope.$index, scope.row)"
                type="danger"
                icon="el-icon-delete"
                circle
              >
              </el-button>
            </template>
          </el-table-column>
        </el-table>
        <el-divider></el-divider>
        <el-button
          class="w-full p-2"
          type="primary"
          plain
          @click="addRowCgpa"
          icon="el-icon-circle-plus-outline"
          >Add Row</el-button
        >
        <div class="flex w-full justify-end items-end">
          <el-button class="my-7 flex" type="primary" @click="submitCgpa"
            ><span class="tracking-widest">Calculate</span></el-button
          >
        </div>
      </el-card>

      <div class="flex box-card mx-auto w-9/12 container mt-2 mb-24">
        <el-card class="w-1/2 flex items-center justify-center mr-1 p-4">
          <h4 class="" style="color: #909399">
            Total Credits:
            <span class="text-2xl ml-2" style="color: #409eff">{{
              finalCredits
            }}</span>
          </h4>
        </el-card>
        <el-card class="w-1/2 flex items-center justify-center ml-1">
          <h4 style="color: #909399">
            CGPA:
            <span class="text-2xl ml-2" style="color: #409eff">{{ cgpa }}</span>
          </h4>
        </el-card>
      </div>
      <div class="flex absolute bottom-3 right-4">
        Developed By
        <a
          href="https://varuntv.netlify.app/"
          target="blank"
          style="text-decoration: none; color: #409eff"
          class="flex flex-col ml-2 items-center justify-center"
          >Varun TV</a
        >
      </div>
    </el-tab-pane>
  </el-tabs>
</template>

<script>
export default {
  data() {
    return {
      tableData: [{}],
      cgpaTableData: [{}],
      grades: [
        {
          value: "10",
          label: "S",
        },
        {
          value: "9",
          label: "A",
        },
        {
          value: "8",
          label: "B",
        },
        {
          value: "7",
          label: "C",
        },
        {
          value: "5",
          label: "D",
        },
        {
          value: "0",
          label: "F",
        },
      ],
      semisters: [
        {
          value: "1",
        },
        {
          value: "2",
        },
        {
          value: "3",
        },
        {
          value: "4",
        },
        {
          value: "5",
        },
        {
          value: "6",
        },
        {
          value: "7",
        },
        {
          value: "8",
        },
      ],
      totalCredits: null,
      sgpa: null,
      finalCredits: null,
      cgpa: null,
      activeName: "sgpa",
    };
  },
  methods: {
    addRow() {
      this.tableData = [...this.tableData, {}];
    },
    addRowCgpa() {
      this.cgpaTableData = [...this.cgpaTableData, {}];
    },
    deleteRow(index) {
      this.tableData.splice(index, 1);
    },
    deleteCgpaRow(index) {
      this.cgpaTableData.splice(index, 1);
    },
    submit() {
      let total = 0;
      let sigma = 0;
      // let totalGradeValue = 0;
      let creditsAndGrade = 0;
      let sgpa = 0;
      let formData = JSON.parse(JSON.stringify(this.tableData));
      console.log("formData", formData);
      formData.forEach((data) => {
        parseFloat((total += parseFloat(data.credits)));
        // parseFloat((totalGradeValue += parseFloat(data.grade)));
        creditsAndGrade = parseFloat(data.grade) * parseFloat(data.credits);
        sigma += creditsAndGrade;
        // console.log("sigma", creditsAndGrade);
      });
      // console.log("ci X si", sigma);
      // console.log(totalGradeValue);
      console.log("total credits", total);
      sgpa = (sigma / total).toFixed(2);
      // console.log("sgpa", sgpa);
      this.totalCredits = total;
      this.sgpa = sgpa;
    },
    submitCgpa() {
      let totalCredits = 0;
      let sigma = 0;
      let creditsAndSgpa = 0;
      let cgpa = 0;
      let formData = JSON.parse(JSON.stringify(this.cgpaTableData));
      console.log("formData", formData);
      formData.forEach((credits) => {
        parseFloat((totalCredits += parseFloat(credits.totalSemCredits)));
        creditsAndSgpa =
          parseFloat(credits.totalSemCredits) * parseFloat(credits.sgpa);
        sigma += creditsAndSgpa;
        // console.log("creditsAndSgpa", creditsAndSgpa);
      });
      // console.log("totalCredits", totalCredits);
      // console.log("sigma", sigma);
      cgpa = (sigma / totalCredits).toFixed(2);
      // console.log("cpga", cgpa);
      this.finalCredits = totalCredits;
      this.cgpa = cgpa;
    },
  },
};
</script>
