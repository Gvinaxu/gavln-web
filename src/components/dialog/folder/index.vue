<template>
  <el-dialog
      class="folder-dialog"
      :visible="visible"
      width="640px"
      :title="title || defaultTitle">
    <el-button-group class="nav">
      <el-button
          type="text">
        <i class="iconfont icon-arrow-l-left"></i>
      </el-button>
      <el-button
          type="text">
        <i class="iconfont icon-arrow-l-right"></i>
      </el-button>
    </el-button-group>
    <el-table
        class="file-table dialog-table"
        :data="data"
        :height="400">
      <el-table-column
          prop="type"
          label="文件名"
          width="68">
        <template>
          <i class="iconfont icon-folder-add"></i>
        </template>
      </el-table-column>
      <el-table-column>
        <template
            slot-scope="scope">
          <a
              :href="`/?path=${scope.row.id}`">{{ scope.row.name }}</a>
        </template>
      </el-table-column>
      <el-table-column
          label="修改时间"
          width="160">
        <template
            slot-scope="scope">
          <span>{{ scope.row.utime | time('yyyy/MM/dd HH:mm') }}</span>
        </template>
      </el-table-column>
    </el-table>
    <div
        slot="footer">
      <el-button
          class="left">新建文件夹</el-button>
      <el-button>取消</el-button>
      <el-button>{{ actionLabel }}</el-button>
    </div>
  </el-dialog>
</template>

<script>
import {
  Dialog,
  ButtonGroup,
  Table,
  TableColumn,
  Button,
} from 'element-ui';

import Mock from '../../../api/mock';

export default {
  name: 'FolderDialog',
  components: {
    'el-dialog': Dialog,
    'el-button-group': ButtonGroup,
    'el-table': Table,
    'el-table-column': TableColumn,
    'el-button': Button,
  },
  props: {
    visible: {
      type: Boolean,
      required: true,
    },
    type: { // move | copy | save
      type: String,
      default: 'move',
    },
    title: String,
  },
  data() {
    return {
      data: Mock.files,
    };
  },
  computed: {
    defaultTitle() {
      const titles = {
        move: '移动文件',
        copy: '复制文件',
        save: '保存到网盘',
      };

      return titles[this.type];
    },
    actionLabel() {
      const labels = {
        move: '移动',
        copy: '复制',
        save: '保存',
      };

      return labels[this.type];
    },
  },
};
</script>

<style
    src="./index.scss"
    lang="scss"
    scoped></style>