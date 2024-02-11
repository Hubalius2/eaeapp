<template>
    <div class="grid grid-cols-2 gap-6 ">
        <CountrySalary />
        <PositionSalary />
        <RemoteRatio />
        <YearSalary />
        <LevelSalary />
        <SalarySizeLocation />
    </div>
</template>

<script setup>
import CountrySalary from './CountrySalary.vue';
import PositionSalary from './PositionSalary.vue';
import RemoteRatio from './RemoteRatio.vue';
import YearSalary from './YearSalary.vue';
import LevelSalary from './LevelSalary.vue';
import SalarySizeLocation from './SalarySizeLocation.vue';

</script>