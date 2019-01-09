<template>
  <nb-container :style="{ backgroundColor: '#fff' }">
        <nb-header>
          <nb-left>
            <nb-button
              transparent
              :onPress="() => this.props.navigation.navigate('DrawerOpen')"
            >
              <nb-icon name="menu" />
            </nb-button>
          </nb-left>
          <nb-body>
            <nb-title>Icons</nb-title>
          </nb-body>
          <nb-right />
        </nb-header>
        <nb-content padder>
            <text>HOLA MUNDO</text>
        </nb-content>
      </nb-container>
</template>


<script>
export default {
  data: function() {
    return {
      listItemArr: [
        {
          route: "BasicIcon",
          text: "Basic Icon"
        },
        {
          route: "Icons",
          text: "Icons"
        },
        {
          route: "PlatformSpecificIcon",
          text: "Platform Specific Icon"
        },
        {
          route: "IconFamily",
          text: "Icon Family"
        }
      ]
    };
  }
};
</script>


<style>
.body-icon-color {
  color: #999;
}
</style>
