<template>
  <div class="w-full">
    <div class="bg-gray container mx-auto">
      <h1 class="text-black text-4xl my-4">Search</h1>
      <input
        v-model="query"
        @input="onInput"
        type="text"
        placeholder="Search..."
        class="border-2 p-4 rounded bg-white w-full"
      />
      <div class="content" v-html="sanitizedContent" />
      </div>
  </div>
</template>

<script setup>
import DOMPurify from 'dompurify';

const content = ref(`
  <h2 class="text-black text-2xl my-5">Understanding between grid tenmpalte and grid-auto</h2>
  <i>Oct 09, 2018</i>
  <p>with all new properties related to CSS Grid Layout, one of the distrinction that always confused me was the difference between the frid-template-* and grid auto-* properties.</p>
  <p>Specially the difference between grid tempate-row/colums and grid-auto-rows/colums.</p>
  <p>although i jnwe they to d...</p>
  <div class="my-8 border border-gray"></div>
  <h2 class="text-black text-2xl mb-5">This is the Lorem Ipsum great example</h2>
  <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Impedit cumque ad soluta nostrum mollitia accusantium laborum consequatur consequuntur, enim repellat earum aspernatur ipsa dolorem accusamus. Molestias similique adipisci ipsum consequuntur?</p>
`);

const query = ref('');
const sanitizedContent = computed(() => {
  const highlightedContent = highlightContent.value;
  return DOMPurify.sanitize(highlightedContent);
});
const highlightContent = computed(() => {
  if (!query.value) return content.value;

const regex = new RegExp(`([^<]*)(${query.value})([^>]*)(<[^>]*>)`, 'gi');

const highlightedText = content.value
    .replace(regex, (match, before, searchTerm, after, tag) => {
      return `${before}<mark>${searchTerm}</mark>${after}${tag}`;
    })
    .replace(/(class=["'])([^"']*?)(["'])/g, (match, p1, p2, p3) => {
      const classRegex = new RegExp(`([^0-9\\W])(${query.value})([^0-9\\W])`, 'gi');
      const highlightedClasses = p2.replace(classRegex, (m, start, searchTerm, end) => {
        return `${start}<mark>${searchTerm}</mark>${end}`;
      });
      return `${p1}${highlightedClasses}${p3}`;
    });

return highlightedText;
});

const onInput = () => {
  highlightContent.value;
};
</script>