<template>
  <v-container fluid class="pa-0 ma-0" style="overflow: auto">
    <splitpanes style="height:calc(100vh - 40px)" class="xc-theme">
      <pane min-size="10" :size="paneSize" max-size="50" style="overflow: auto">
        <ProjectTreeView ref="treeview"/>
      </pane>
      <pane :size="100 - paneSize">

        <ProjectTabs :key="pid" @tableCreate="tableCreate"/>
      </pane>
    </splitpanes>

  </v-container>
</template>
<script>

import {Splitpanes, Pane} from 'splitpanes'
import ProjectTabs from "@/components/projectTabs";
import ProjectTreeView from "@/components/ProjectTreeView";


export default {
  components: {
    ProjectTreeView,
    ProjectTabs,
    Splitpanes,
    Pane,
  },
  data() {
    return {
      paneSize: 18,
      mainPanelSize: 82
    };
  },
  async created() {
    this.$store.watch(
      (state) => state.panelSize.treeView && state.panelSize.treeView.size,
      (newSize) => this.paneSize = newSize
    )


  },

  mounted() {
    if ('new' in this.$route.query) {
      this.simpleAnim()
      this.$router.replace({query: {}})
    }
    try {
      hj('stateChange', 'http://localhost:8080/dashboard/#/nc/');
    } catch (e) {
    }
  },
  methods: {
    tableCreate(table) {
      if (this.$refs.treeview) {
        this.$refs.treeview.mtdTableCreate(table);
      }
    },
    simpleAnim() {
      var count = 200;
      var defaults = {
        origin: {y: 0.7}
      };

      function fire(particleRatio, opts) {
        window.confetti(Object.assign({}, defaults, opts, {
          particleCount: Math.floor(count * particleRatio)
        }));
      }

      fire(0.25, {
        spread: 26,
        startVelocity: 55,
      });
      fire(0.2, {
        spread: 60,
      });
      fire(0.35, {
        spread: 100,
        decay: 0.91,
        scalar: 0.8
      });
      fire(0.1, {
        spread: 120,
        startVelocity: 25,
        decay: 0.92,
        scalar: 1.2
      });
      fire(0.1, {
        spread: 120,
        startVelocity: 45,
      });
    },
  },
  computed: {
    pid() {
      return this.$route.params.project_id
    }
  }
};
</script>
<style scoped>
/deep/ .splitpanes__splitter {
  background: #7f828b33 !important;
  border: #7f828b33 !important;
}
</style>
<!--
/**
 * @copyright Copyright (c) 2021, Xgene Cloud Ltd
 *
 * @author Naveen MR <oof1lab@gmail.com>
 * @author Pranav C Balan <pranavxc@gmail.com>
 *
 * @license GNU AGPL version 3 or any later version
 *
 * This program is free software: you can redistribute it and/or modify
 * it under the terms of the GNU Affero General Public License as
 * published by the Free Software Foundation, either version 3 of the
 * License, or (at your option) any later version.
 *
 * This program is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 * GNU Affero General Public License for more details.
 *
 * You should have received a copy of the GNU Affero General Public License
 * along with this program. If not, see <http://www.gnu.org/licenses/>.
 *
 */
-->
