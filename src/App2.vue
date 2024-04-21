<script setup>

import { airports, flights } from '@2bad/ryanair';

const dates = flights.getDates('BER', 'DUB')


</script>




<template>
    
    <div>
        {{ dates }}
    </div>

</template>