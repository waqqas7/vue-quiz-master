<template>
    <div>
        <h1>Sports Quiz Master by Waqqas</h1>
        <b-nav tabs>
            <b-nav-item disabled><b>This quiz contains ten questions</b></b-nav-item>
            <b-nav-item disabled>Score: {{ numCorrect }}/{{ numTotal }}</b-nav-item>
        </b-nav>
    </div>
</template>

<script>
    export default {
        props: [
            'numCorrect',
            'numTotal'
        ]
    }

</script>