<script setup lang="ts">

let tabSeparator = "%-tab-%"
let lineSeparator = "%-line-%"
let quizletInput = ""

function convertToCSV(){
    let quizletArray: string[][] = []
    let quizletInputArray = quizletInput.split(lineSeparator)
    for (const index in quizletInputArray) {
        if (quizletInputArray[index] === "") {
            continue
        }
        quizletArray.push(quizletInputArray[index].split(tabSeparator))
    }
    console.log(quizletArray)
    downloadCSV(export2DArrayToCSV(quizletArray), "quizlet-flashcard.csv")
}

// function export2DArrayToCSV(array: string[][]): string {
//     const csvLines = array.map(row => {
//         const sanitizedRow = row.map(item => {
//             if (typeof item === 'string') {
//                 return item.replace(/\n/g, '\\n');
//             }
//             return item;
//         });
//         return sanitizedRow.join(',');
//     });
//
//     return csvLines.join('\n');
// }

function export2DArrayToCSV(array: any[][]): string {
    const csvLines = array.map(row => {
        const sanitizedRow = row.map(item => {
            if (typeof item === 'string') {
                if (item.includes('"') || item.includes(',') || item.includes('\n')) {
                    return `"${item.replace(/"/g, '""')}"`;
                }
            }
            return item;
        });

        return sanitizedRow.join(',');
    });

    return csvLines.join('\n');
}

function downloadCSV(csvContent: string, fileName: string): void {
    const csvData = new Blob([csvContent], { type: 'text/csv' });
    const csvUrl = URL.createObjectURL(csvData);

    const downloadLink = document.createElement('a');
    downloadLink.href = csvUrl;
    downloadLink.download = fileName;

    document.body.appendChild(downloadLink);
    downloadLink.click();

    document.body.removeChild(downloadLink);
}
</script>

<template>
  <h1>Quizlet CSV exporter</h1>

    <div class="v-row w-75 mx-auto mt-10">
        <ol class="w-auto mx-auto mt-10 text-left">
            <li>open your Quizlet-flashcards in your browser</li>
            <li>open the export-modal (<a target="_blank" href="https://help.quizlet.com/hc/en-us/articles/360034345672-Exporting-your-sets">here more infos</a>)</li>
            <li>choose "User defined" and set the same characters as in the lower input fields</li>
            <li>copy this text and insert it here</li>
        </ol>
    </div>
    <div class="v-row w-75 mx-auto mt-10">
        <v-text-field
                v-model="tabSeparator"
                label="Tab Separator"
                required
                class="v-50"
        ></v-text-field>
        <div class="mx-5"></div>
        <v-text-field
                v-model="lineSeparator"
                label="Line Separator"
                required
                class="v-50"
        ></v-text-field>
    </div>
    <div class="v-row w-75 mx-auto mt-10">
        <v-textarea
                label="Quizlet export text"
                @input="convertToCSV()"
                v-model="quizletInput"
                required
        ></v-textarea>
    </div>

</template>

<style scoped></style>
