<script lang="ts">
    import sourceLang from "../consts/sourceLang.json";
    import targetLang from "../consts/targetLang.json";
    export let placeholder: undefined | string = undefined;
    export let isSource: boolean = false;
    export let isDisabled: boolean = false;
    let counter = 0;
    const updateCounter = (event: Event) => {
        const target = event.target as HTMLTextAreaElement;
        counter = target.value.length;
    };

    const handleOnInput = (event: Event) => {
        updateCounter(event);
    };
</script>

<div class="translator-box">
    <select name={isSource ? "sourceLang" : "targetLang"}>
        {#each isSource ? sourceLang : targetLang as language}
            <option value={language.short}>{language.long}</option>
        {/each}
    </select>
    <div>
        <textarea
            disabled={isDisabled}
            name={isSource ? "text" : undefined}
            placeholder={placeholder}
            maxlength={250}
            on:input={handleOnInput}
        />
        <div class="character-counter">
            <small style={`color:${counter === 250 ? "red" : "black"};`}
                >{counter}/250</small
            >
        </div>
    </div>
</div>
