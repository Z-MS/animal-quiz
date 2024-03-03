<script>
  import { _, locale } from "svelte-i18n";
  
  export let currentLocale = "en";
  
  function changeLocale(event) {
    event.preventDefault();
    $locale = event.target.value;
  }
</script>

<div>
    <select value={currentLocale} on:change={changeLocale}>
        <option value="en">{$_('locale_switcher_english')}</option>
        <option value="fr">{$_('locale_switcher_french')}</option>
    </select>
</div>