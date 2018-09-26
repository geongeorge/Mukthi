<template>
    <div>
        <div class="button is-large is-fullwidth is-my-btn" @click="clickedd" :class="imgtypes[type].color">
 
                <img :src="imgtypes[type].img" alt="" class="mineimg">

                {{imgtypes[type].text}}

            <slot></slot>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        type: {default: 'camp'}
    },
    data() {
        return {
            imgtypes : {
                camp: {
                    img: require("../assets/icons/camp.png"),
                    color: "is-primary",
                    text: "Camps"
                },
                collection: {
                   img:  require("../assets/icons/collection.png"),
                   color: "is-warning",
                   text: "Collection Centers"
                },
                road: {
                   img: require("../assets/icons/road.png"),
                   color: "is-danger",
                   text: "Road Blocks"
                },
                help: {
                    img: require("../assets/icons/help.png"),
                    color: "is-success",
                    text: "Help Needed"
                }
            }
        }
    },
    methods: {
        clickedd() {
            this.$emit("wasclicked",this.type);
        }
    }
}
</script>

<style>
.button {
  height: 50px;
  border-radius: 0!important;
  transition: height ease 0.5s;
}
.button:hover {
  height: 70px;
}
.is-my-btn {
    font-size: 1.5rem!important;
}
.mineimg {
    display:inline-block;
    max-width: 30px;
    margin-right: 5px;
}
</style>
