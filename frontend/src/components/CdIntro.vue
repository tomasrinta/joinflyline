<template>
  <div class="newhome-form" id="newhome-form">
    <h1 class="newhome-form__title" v-if="$mq === 'sm'">
      Spend Less, Travel More
    </h1>
    <h1 class="newhome-form__title" v-else>
      Save Time & Money with FlyLine
    </h1>
    <h5 class="newhome-form__subtitle">
      Your direct line to a comprehensive flight search with exclusive fares and
      zero markup
    </h5>
    <!-- <h3>Spend Less,<br/> Travel More -->
    <!-- <div class="cd-intro full-web-form">
                <h6 class="cd-headline slide loading-bar">
                <span class="cd-words-wrapper">
                    <b class="is-visible">Weekend Getaways</b>
                    <b>Business Travel</b>
                    <b>Visiting Family</b>
                    <b>Last Minute Trips</b>
                    <b>International Travel</b>
                    <b>Summer Vacation</b>
                    <b>The Holidays</b>
                    <b>Wedding Season</b>
                </span>
                </h6>
            </div> -->
    <!-- cd-intro -->
    <!-- </h3> -->
    <!-- <p>Book travel for all occasions and save</p>
        <span>Spend Less,<br/>Travel More</span> -->
  </div>
</template>

<script>
export default {
  delimiters: ["{{", "}}"]
};
</script>
