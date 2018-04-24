<template scope="props">
    <div>
        <p>hello world {{isNumber}}</p>
        <slot name="header">
            只有在没有要分发的内容时才会显示。
        </slot>
        <slot>
            只有在没有要分发的内容时才会显示。
        </slot>

        <slot name="footer">
            尾部内容
        </slot>
    </div>
</template>

<script>
export default {
  props: ['isNumber'],
}
</script>

<style>

</style>
