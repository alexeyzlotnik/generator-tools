<template>
  <UContainer>
    <UPage>
      <UPageHeader
        title="Рассчет эллиптического и полусферического днища"
        description="Он-лайн калькулятор расчета эллиптического и полусферического днища дает возможность оперативно провести предварительный расчет на прочность по ГОСТ 34233.2-2017. Допускаемые напряжения определяются автоматически по ГОСТ 34233.1-2017."
        :ui="{title: 'md:text-center w-full', description: 'md:text-center w-full', wrapper: 'border-0'}"
      />
      <UPageBody class="flex flex-col gap-12">
        <ZlotnikCalculation />
      </UPageBody>
    </UPage>
  </UContainer>
</template>

<script setup>

</script>

<style>

</style>