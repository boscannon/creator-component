<script setup>
import { ref } from 'vue'
import 'element-plus/dist/index.css'
import { ElUpload, ElButton, ElIcon, ElDialog } from 'element-plus';
import { Plus } from '@element-plus/icons-vue'

const props = defineProps({
    modelValue: { type: [String, Array, Object], default: null },
    multiple: { type: Boolean, default: false },
    accept: { type: String, default: null },
    autoUpload: { type: Boolean, default: false },
})

const upload = ref(null)
const fileList = props.multiple ? props.modelValue?.map(item => ({ id: item.id, name: item.name, url: item.original_url })) : [{ url: props.modelValue }];
const imageUrl = ref(props.multiple ? '' : props.modelValue)

const dialogImageUrl = ref('')
const dialogVisible = ref(false)

const emit = defineEmits(['update:modelValue', 'updateDelete'])

const changeData = (list) => {
    if(props.multiple){
        emit('update:modelValue', list.map(item => item.raw))
    }else{
        emit('update:modelValue', list?.pop().raw)
    }
}

const onChange = (response, list) => {
    if(!props.multiple){
        imageUrl.value = response.url;
    }
    changeData(list);
};

const onRemove = (data, list) => {
    changeData(list);
    if(data.id){
        emit('updateDelete', data.id)
    }
};

const handlePictureCardPreview = (uploadFile) => {
    dialogImageUrl.value = uploadFile.url
    dialogVisible.value = true
}

</script>

<template>
    <el-upload
        ref="upload"
        v-model:file-list="fileList"
        multiple
        :show-file-list="multiple ? true : false"
        :accept="accept"
        :action="route('backend.upload.store')"
        list-type="picture-card"
        :on-preview="handlePictureCardPreview"
        :on-change="onChange"
        :on-remove="onRemove"
        :auto-upload="autoUpload"
    >
        <img v-if="imageUrl" :src="imageUrl" class="avatar" />
        <el-icon v-else class="avatar-uploader-icon"><Plus /></el-icon>
    </el-upload>

    <el-dialog v-model="dialogVisible">
        <img w-full :src="dialogImageUrl" alt="Preview Image" />
    </el-dialog>
</template>
