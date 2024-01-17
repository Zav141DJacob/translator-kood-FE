<script lang="ts">
    import { Link } from "svelte-routing";
    import { BACKEND } from "../consts/other";
    import TranslatorBox from "../lib/TranslatorBox.svelte";
    import targetLangs from "../consts/targetLang.json";
    import sourceLangs from "../consts/sourceLang.json";


    let previous = 0;
    let isLoading = false;
    let placeholder = "Translation";

    const handleSubmit = (event: Event) => {
        event.preventDefault();
        isLoading = true;


        const target = event.target as HTMLFormElement;
        const formBody = {
            text: target.text.value.trim(),
            sourceLang: target.sourceLang.value,
            targetLang: target.targetLang.value,
        };

        if (formBody.text.trim().length === 0) {
            placeholder = "Text field can not be empty"
            isLoading = false
            return
        }

        fetch(`${BACKEND}/translate`, {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify(formBody),
        }).then((resp: Response) => {
            if (resp.ok) {
                resp.json().then((jsonV) => {
                    // Loading text removed and replaced with translated text
                    isLoading = false;
                    placeholder = jsonV.text;
                    
                    // Fetch history from localStorage
                    const history = localStorage.getItem("history");

                    // Create history record using local date
                    const currentDate = new Date();
                    const formattedDate = currentDate.toLocaleString("en-US", {
                        year: "numeric",
                        day: "numeric",
                        month: "long",
                        minute: "numeric",
                        hour: "numeric",
                        hourCycle: "h24",
                    });

                    // Update history inside localStorage
                    localStorage.setItem(
                        "history",
                        JSON.stringify([
                            {
                                text: target.text.value.trim(),
                                translation: jsonV.text,
                                sourceLang: sourceLangs.find((elem) => elem.short === target.sourceLang.value)?.long,
                                targetLang: targetLangs.find((elem) => elem.short === target.targetLang.value)?.long,
                                date: formattedDate,
                            },
                            ...(history ? JSON.parse(history) : []),
                        ]),
                    );
                });
            }
        });
    };
</script>

<div>
    <div id="translator-and-history">
        <Link to="/history" class="link" id="history-link">History</Link>
        <form class="down-shadow" id="translator-widget" on:submit={handleSubmit}>
            <div id="translator-boxes">
                <TranslatorBox isSource placeholder="Enter text" />
                <div class="svg-container">
                    <svg
                        id="right-arrow"
                        class="icon"
                        xmlns="http://www.w3.org/2000/svg"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke-width="1.5"
                        stroke="currentColor"
                    >
                        <path
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            d="M17.25 8.25 21 12m0 0-3.75 3.75M21 12H3"
                        />
                    </svg>
                </div>
                <TranslatorBox
                    isDisabled
                    placeholder={isLoading ? "Translating..." : placeholder}
                />
            </div>
            <div class="center-width">
                <button class="translate-button">Translate</button>
            </div>
        </form>
    </div>
</div>
