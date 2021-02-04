<script>
  import { Pagination, PaginationItem, PaginationLink } from 'sveltestrap'
  import { createEventDispatcher } from 'svelte'
  import generateNavigationOptions from './generateNavigationOptions'
  import {
    PREVIOUS_PAGE,
    NEXT_PAGE,
    ELLIPSIS
  } from '../src/symbolTypes'

  const dispatch = createEventDispatcher()

  export let totalItems = 0
  export let pageSize = 1
  export let currentPage = 1
  export let limit = null
  export let showStepOptions = false

  $: options = generateNavigationOptions({
    totalItems,
    pageSize,
    currentPage,
    limit,
    showStepOptions
  })

  $: totalPages = Math.ceil(totalItems / pageSize)

  function handleOptionClick (option) {
    dispatch('setPage', { page: option.value })
  }
</script>

<Pagination>
  {#each options as option}
    <PaginationItem
      active={option.value == currentPage}
      disabled={
        (option.type === 'symbol' && option.symbol === NEXT_PAGE && currentPage >= totalPages) ||
        (option.type === 'symbol' && option.symbol === PREVIOUS_PAGE && currentPage <= 1)
      }
    >
      <PaginationLink
        href="#"
        on:click="{() => handleOptionClick(option)}"
      >
        {#if option.type === 'number'}
          {option.value}
        {:else if option.type === 'symbol' && option.symbol === ELLIPSIS}
          ...
        {:else if option.type === 'symbol' && option.symbol === PREVIOUS_PAGE}
          {'\u2039'}
        {:else if option.type === 'symbol' && option.symbol === NEXT_PAGE}
          {'\u203A'}
        {/if}

      </PaginationLink>
    </PaginationItem>
  {/each}
</Pagination>