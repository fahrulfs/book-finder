<template>
    <div class="flex flex-col md:flex-row rounded-md gap-4 border border-yellow-500 p-3 h-fit bg-black w-full">

        <div>
            <slot name="header" class="w-1/2"/>
        </div>
      
        <div>
            <slot name="content" class="w-1/2" />
        </div>
    </div>
</template>