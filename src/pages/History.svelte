<script lang="ts">
    import { Link } from "svelte-routing";

    const local = localStorage.getItem("history");
    const history = local ? JSON.parse(local) : [];
</script>

<!-- https://freefrontend.com/tailwind-tables/ -->
<!-- https://tailwindcomponents.com/component/table-visits -->
<div id="table-with-link">
    <Link to="/" class="link">Main page</Link>

    <div class="page-table-container">
        <table class="page-table">
            <thead>
                <tr>
                    <th class="page-table-header"> Text </th>
                    <th class="page-table-header"> Translation </th>
                    <th class="page-table-header"> Source language </th>
                    <th class="page-table-header"> Target language </th>
                    <th class="page-table-header"> Date </th>
                </tr>
            </thead>

            <tbody>
                {#each history as { text, translation, sourceLang, targetLang, date }}
                    <tr>
                        <td class="page-table-cell big-cell">{text}</td>
                        <td class="page-table-cell big-cell">{translation}</td>
                        <td class="page-table-cell">{sourceLang}</td>
                        <td class="page-table-cell">{targetLang}</td>
                        <td class="page-table-cell">{date} </td>
                    </tr>
                {/each}
            </tbody>
        </table>
    </div>
</div>

<style>
    #table-with-link {
        padding: 10% 5%;
        display: flex;
        flex-direction: column;
        gap: 0.5rem;
    }
    .big-cell {
        width: 35%;
    }
    .page-table-container {
    }
    .page-table {
        border: 1px solid rgba(188, 224, 255, 0.312);

        align-items: center;
        width: 100%;
        border-collapse: collapse;
        box-shadow:
            4.2px 2.6px 2.2px rgba(0, 0, 0, 0.02),
            10px 6.3px 5.3px rgba(0, 0, 0, 0.028),
            18.9px 11.9px 10px rgba(0, 0, 0, 0.035),
            33.7px 21.2px 17.9px rgba(0, 0, 0, 0.042),
            63.1px 39.7px 33.4px rgba(0, 0, 0, 0.05),
            151px 95px 80px rgba(0, 0, 0, 0.07);
    }

    .page-table-cell {
        box-shadow: 0px 5.1px 2.2px rgba(0, 0, 0, 0.07);
        border: 1px solid rgba(188, 224, 255, 0.312);

        min-width: 11rem;
        max-width: 20rem;

        border-top: 0;
        padding: 0.75rem;
        border-left: 0;
        border-right: 0;
        vertical-align: top;
        text-align: left;
        font-size: 1rem;
        word-wrap: break-word;
    }

    .page-table-header {
        padding: 0.75rem;
        background-color: aliceblue; /* Replace with your desired background color */
        color: black;
        border: 1px solid rgb(188, 224, 255);
        text-align: left;
        font-weight: 600;
        font-size: 0.75rem;
        text-transform: uppercase;
        white-space: nowrap;
    }
</style>
