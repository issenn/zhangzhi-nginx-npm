<template>
  <div>
    <Icon type="cube" size="40" class="MenuIcon"></Icon>
    <template v-for="(item, index) in menuList">
      <Dropdown v-if="item.children.length >= 1 && item.name != 'main'  && $store.state.filtermenulist[item.name] === 1" placement="right-start" :key="index"
                 @on-click="changeMenu">
         <Button style="width: 70px;margin-left: -5px;padding:10px 0;" type="text">
           <Icon :size="20" :color="iconColor" :type="item.icon"></Icon>
         </Button>
         <DropdownMenu style="width: 200px;" slot="list">
           <template v-for="child in item.children">
             <template v-if="$store.state.filtermenulist[child.name] === 1">
               <DropdownItem :name="child.name" :key="child.title">
                 <Icon :type="child.icon"></Icon>
                 <span style="padding-left:10px;">{{ child.title }}</span>
               </DropdownItem>
             </template>
           </template>
         </DropdownMenu>
       </Dropdown>
     </template>
     <Dropdown placement="right-start" @on-click="changeMenu">
       <Button @click="changeMenu('login')" style="width: 70px;margin-left: -5px;padding:10px 0;" type="text">
         <Icon type="log-out" size="20" :color="iconColor"></Icon>
       </Button>
       <DropdownMenu slot="list">
         <DropdownItem name="login" key="login">退出</DropdownItem>
       </DropdownMenu>
     </Dropdown>
   </div>
</template>

<script>
  //
  import util from '../libs/util'
  // import axios from 'axios'

  export default {
    name: 'sidebarMenuShrink',
    props: {
      menuList: {
        type: Array
      },
      iconColor: {
        type: String,
        default: 'white'
      }
    },
    data () {
      return {
        currentPageName: this.$route.name,
        openedSubmenuArr: this.$store.state.openedSubmenuArr
        // filtermenulist: {
        //   'management-user': '1',
        //   'management-project': '1',
        //   'management-game': '1',
        //   'management-host': '1'
        // }
      }
    },
    methods: {
      changeMenu (active) {
        if (active === 'login') {
          localStorage.clear()
          sessionStorage.clear()
          this.$router.push({
            name: active
          })
        } else {
          util.openPage(this, active)
        }
      }
    },
    created () {
        console.log(this.$store.state.filtermenulist, 222222)
        console.log(this.menuList, 333333)
      // axios.get(`${util.url}/homedata/menu`)
      //   .then(res => {
      //     let c = JSON.parse(res.data)
      //     this.filtermenulist.ddledit = c.ddl
      //     this.filtermenulist.indexedit = c.ddl
      //     this.filtermenulist.dmledit = c.dml
      //     this.filtermenulist['view-dml'] = c.dic
      //     this.filtermenulist['management-user'] = c.user
      //     this.filtermenulist['management-database'] = c.base
      //   })
    }
  }
</script>
