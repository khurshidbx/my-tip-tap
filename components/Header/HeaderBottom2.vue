<template>
    <div>
        <ul class="bg-[] border-b border-b-solid py-1 px-2 flex items-center gap-3 flex-wrap">
            <li>
                <button title="insertTable" @click="$emit('trig-table', 'insertTable')"
                    class="flex items-center gap-2 py-1 px-3 transition hover:bg-[#edf5ef] rounded">
                    <span class="">insertTable</span>
                </button>
            </li>
            <li>
                <button title="deleteTable" @click="$emit('trig-table', 'deleteTable')"
                    class="flex items-center gap-2 py-1 px-3 transition hover:bg-[#edf5ef] rounded">
                    <span class="">deleteTable</span>
                </button>
            </li>
            <li>
                <button title="addColumnBefore"
                    class="flex items-center gap-2 py-1 px-3 transition hover:bg-[#edf5ef] rounded"
                    @click="$emit('trig-table', 'addColumnBefore')">
                    <span class="">addColumnBefore</span>
                </button>
            </li>
            <li>
                <button title="addColumnAfter"
                    class="flex items-center gap-2 py-1 px-3 transition hover:bg-[#edf5ef] rounded"
                    @click="$emit('trig-table', 'addColumnAfter')">
                    <span class="">addColumnAfter</span>
                </button>
            </li>
            <li>
                <button title="deleteColumn"
                    class="flex items-center gap-2 py-1 px-3 transition hover:bg-[#edf5ef] rounded"
                    @click="$emit('trig-table', 'deleteColumn')">
                    <span class="">deleteColumn</span>
                </button>
            </li>
            <li>
                <button title="addRowBefore"
                    class="flex items-center gap-2 py-1 px-3 transition hover:bg-[#edf5ef] rounded"
                    @click="$emit('trig-table', 'addRowBefore')">
                    <span class="">addRowBefore</span>
                </button>
            </li>
            <!-- <li>
                <button title="addRowBefore"
                    class="flex items-center gap-2 py-1 px-3 transition hover:bg-[#edf5ef] rounded"
                    @click="$emit('trig-table', 'addRowBefore')">
                    <span class="">addRowBefore</span>
                </button>
            </li> -->
            <li>
                <button title="addRowAfter"
                    class="flex items-center gap-2 py-1 px-3 transition hover:bg-[#edf5ef] rounded"
                    @click="$emit('trig-table', 'addRowAfter')">
                    <span class="">addRowAfter</span>
                </button>
            </li>
            <li>
                <button title="deleteRow"
                    class="flex items-center gap-2 py-1 px-3 transition hover:bg-[#edf5ef] rounded"
                    @click="$emit('trig-table', 'deleteRow')">
                    <span class="">deleteRow</span>
                </button>
            </li>
        </ul>




        <div v-if="editor">
            <button class="border border-sky-500"
                @click="editor.chain().focus().insertTable({ rows: 3, cols: 3, withHeaderRow: true }).run()">
                insertTable
            </button>
            <button @click="editor.chain().focus().addColumnBefore().run()" :disabled="!editor.can().addColumnBefore()">
                addColumnBefore
            </button>
            <button @click="editor.chain().focus().addColumnAfter().run()" :disabled="!editor.can().addColumnAfter()">
                addColumnAfter
            </button>
            <button @click="editor.chain().focus().deleteColumn().run()" :disabled="!editor.can().deleteColumn()">
                deleteColumn
            </button>
            <button @click="editor.chain().focus().addRowBefore().run()" :disabled="!editor.can().addRowBefore()">
                addRowBefore
            </button>
            <button @click="editor.chain().focus().addRowAfter().run()" :disabled="!editor.can().addRowAfter()">
                addRowAfter
            </button>
            <button @click="editor.chain().focus().deleteRow().run()" :disabled="!editor.can().deleteRow()">
                deleteRow
            </button>
            <button @click="editor.chain().focus().deleteTable().run()" :disabled="!editor.can().deleteTable()">
                deleteTable
            </button>
            <button @click="editor.chain().focus().mergeCells().run()" :disabled="!editor.can().mergeCells()">
                mergeCells
            </button>
            <button @click="editor.chain().focus().splitCell().run()" :disabled="!editor.can().splitCell()">
                splitCell
            </button>
            <button @click="editor.chain().focus().toggleHeaderColumn().run()"
                :disabled="!editor.can().toggleHeaderColumn()">
                toggleHeaderColumn
            </button>
            <button @click="editor.chain().focus().toggleHeaderRow().run()" :disabled="!editor.can().toggleHeaderRow()">
                toggleHeaderRow
            </button>
            <button @click="editor.chain().focus().toggleHeaderCell().run()"
                :disabled="!editor.can().toggleHeaderCell()">
                toggleHeaderCell
            </button>
            <button @click="editor.chain().focus().mergeOrSplit().run()" :disabled="!editor.can().mergeOrSplit()">
                mergeOrSplit
            </button>
            <button @click="editor.chain().focus().setCellAttribute('backgroundColor', '#FAF594').run()"
                :disabled="!editor.can().setCellAttribute('backgroundColor', '#FAF594')">
                setCellAttribute
            </button>
            <button @click="editor.chain().focus().fixTables().run()" :disabled="!editor.can().fixTables()">
                fixTables
            </button>
            <button @click="editor.chain().focus().goToNextCell().run()" :disabled="!editor.can().goToNextCell()">
                goToNextCell
            </button>
            <button @click="editor.chain().focus().goToPreviousCell().run()"
                :disabled="!editor.can().goToPreviousCell()">
                goToPreviousCell
            </button>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        editor: {
            type: Object,
            required: true,
        }
    },

    data() {

    },


}
</script>

<style lang="scss" scoped></style>