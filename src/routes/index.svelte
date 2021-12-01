<script>
  import { init } from '@graphcms/uix-sdk'
  import { onMount } from 'svelte'

  let graphCmsOnChange = () => {}
  let value = ''

  // https://graphcms.com/docs/ui-extensions/libraries/javascript-sdk
  onMount(() => {
    init({
      declaration: {
        extensionType: 'field',
        fieldType: 'STRING',
        name: 'My first custom input',
        description: '',
        features: ['FieldRenderer'],
        config: { APIID: { type: 'string', displayName: 'API ID' } },
      },
      // onProps: (newProps) => {
      // 	// newProps is now typed
      // 	const { value } = newProps;
      // 	myInput.value = value;
      // }
    }).then(initialState => {
      const { status, props } = initialState
      if (status === 'ok') {
        const { value: graphCmsValue, onChange } = props
        graphCmsOnChange = onChange
        value = graphCmsValue
        console.log(`Custom field initialized with value ${value}`)
      }
    })
  })

  $: graphCmsOnChange(value)
</script>

<input bind:value type="input" />
