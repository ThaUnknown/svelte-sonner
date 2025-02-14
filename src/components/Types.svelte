<script lang="ts">
import { toast } from "$lib";
    import CodeBlock from "./CodeBlock.svelte";
import Custom from "./Custom.svelte";

const promiseCode = '`${data.name} toast has been added`'

const allTypes = [
  {
    name: 'Default',
    snippet: 'toast(\'Event has been created\')',
    action: () => toast('Event has been created'),
  },
  {
    name: 'Description',
    snippet: `toast.message('Event has been created', {
  description: 'Monday, January 3rd at 6:00pm',
})`,
    action: () =>
      toast('Event has been created', {
        description: 'Monday, January 3rd at 6:00pm',
      }),
  },
  {
    name: 'Success',
    snippet: 'toast.success(\'Event has been created\')',
    action: () => toast.success('Event has been created'),
  },
  {
    name: 'Error',
    snippet: 'toast.error(\'Event has not been created\')',
    action: () => toast.error('Event has not been created'),
  },
  {
    name: 'Action',
    snippet: `toast('Event has been created', {
  action: {
    label: 'Undo',
    onClick: () => console.log('Undo')
  },
})`,
    action: () =>
      toast.message('Event has been created', {
        action: {
          label: 'Undo',
          // eslint-disable-next-line no-console
          onClick: () => console.log('Undo'),
        },
      }),
  },
  {
    name: 'Promise',
    snippet: `const promise = () => new Promise((resolve) => setTimeout(resolve, 2000));

toast.promise(promise, {
  loading: 'Loading...',
  success: (data) => {
    return ${promiseCode};
  },
  error: 'Error',
});`,
    action: () =>
      toast.promise<{ name: string }>(
        () =>
          new Promise((resolve) => {
            setTimeout(() => {
              resolve({ name: 'Svelte Sonner' })
            }, 2000)
          }),
        {
          loading: 'Loading...',
          success: (data) => {
            return `${data.name} toast has been added`
          },
          error: 'Error',
        },
      ),
  },
  {
    name: 'Custom',
    snippet: `import Custom from './Custom.svelte'

toast(Custom)`,
    action: () => toast(Custom),
  },
]

let activeType = allTypes[0]
</script>

<div>
  <h2>Types</h2>
  <p>You can customize the type of toast you want to render, and pass an options object as the second argument.</p>
  <div class="buttons">
    {#each allTypes as type}
      <button
        class="button"
        data-testid={type.name}
        data-active={activeType?.name === type.name}
        on:click={() => {
          type.action?.()
          activeType = type
        }}
      >
        {type.name}
      </button>
    {/each}
  </div>
  <CodeBlock code={activeType?.snippet} />
</div>
