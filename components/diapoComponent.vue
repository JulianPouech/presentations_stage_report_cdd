<script setup lang="ts">
import ReferenceComponent from './ReferenceComponent.vue';

    const props = defineProps({
        title: String,
        reference: String,
    });
</script>

<template>
    <PagesComponent/>
    <ReferenceComponent>{{props.reference}}</ReferenceComponent>
    <div>
        <h1>{{props.title}}</h1>
        <slot/>
    </div>
</template>
