<script>
    import { resultsStore } from "../store.js";
    /*     import { onMount } from "svelte";*/
    import axios from "axios";

    let constellations = [
        "ORI",
        "SCO",
        "LEO",
        "LYR",
        "CYG",
        "AQL",
        "SGE",
        "VUL",
    ];
    let selectedConstellation = constellations[0];

    async function fetchStars() {
        // api url is constructed from docker-compose envs:
        const API_URL = `${import.meta.env.VITE_API_BASE_URL}/constellation?constellation=${selectedConstellation}`;
        console.log(API_URL);

        try {
            const response = await axios.get(API_URL);
            resultsStore.set(response.data);
        } catch (error) {
            console.error("error fetching star data:", error);
        }
    }

    /* onMount(() => {
        fetchStars();
    }); */

    $: selectedConstellation, fetchStars();
</script>

<label for="constellationSelect">Wähle eine Konstellation:</label>

<select id="constellationSelect" bind:value={selectedConstellation}>
    {#each constellations as constellation}
        <option value={constellation}>{constellation}</option>
    {/each}
</select>
