<script>
    let person = "Joku Jokunen";
    let dayFines = [
        { quantity: 1, amount: 6.0, paid: 0.0, unpaid: 1, fver: "Pending" },
    ];

    function addDayFine() {
        dayFines = [
            ...dayFines,
            { quantity: 1, amount: 6.0, paid: 0.0, unpaid: 1, fver: "Pending" },
        ];
    }

    /**
     * @param {number} index
     */
    function removeDayFine(index) {
        dayFines = dayFines.filter((_, i) => i !== index);
    }

    let threatFines = [{ quantity: 0, paid: 0.0, fver: "Pending" }];

    function addThreatFine() {
        threatFines = [...threatFines, { quantity: 0, paid: 0, fver: "Pending" }];
    }

    /**
     * @param {number} index
     */
    function removeThreatFine(index) {
        threatFines = threatFines.filter((_, i) => i !== index);
    }

    let pverdict = "Pending";
    let ttotal = 0;
    let tverdict = "Pending";
    let totalver = 0;

    $: {
        dayFines.forEach((fine) => {
            let unpaid = fine.quantity * fine.amount - fine.paid;
            let unpaidqty = Math.ceil(unpaid / fine.amount);
            
            let numb = unpaidqty ? unpaidqty : 0;
            fine.unpaid = numb;
            fine.fver = "Maksamattomia sakkoja: " + numb + " kpl";
        });

        let newDays = dayFines.filter((fine) => fine.unpaid >= 16);

        let sum = 0;
        newDays.forEach((fine) => {
            sum += fine.unpaid;
        });

        sum = sum / 4;
        sum = Math.floor(sum);

        if (sum < 4) {
            sum = 0
        } else if (sum > 40) {
            sum = 40;
        }

        pverdict = sum + " päivää.";
        totalver = sum;

        // Uhkasakot

        let tsum = 0;
        threatFines.forEach((fine) => {
            let unpaid = fine.quantity - fine.paid;

            fine.fver = "Sakkoa maksamatta: " + unpaid + " €";

            tsum += unpaid;
        });
        ttotal = tsum;

        if (tsum < 120){
            tverdict = 0 + " päivää."
        } else {
            let final = tsum / 30;
            final = Math.floor(final);
            if (final < 4) {
                final = 0
            } else if (final > 40) {
                final = 40;
            }
            tverdict = final + " päivää."


            totalver += final;
            if (totalver > 40) {
                totalver = 40;
            }
        }
    }
</script>

<main>
    <h1>Sakonmuuntolaskuri</h1>

    <input
        type="text"
        class="person"
        placeholder="Person"
        bind:value={person}
    />

    {#each dayFines as fine, index}
        <div class="fine-entry">
            <div class="fine-column">
                <label for="">Päiväsakkojen lukumäärä</label>

                <input
                    type="number"
                    bind:value={fine.quantity}
                    min="1"
                    placeholder="Quantity"
                />
            </div>
            <div class="fine-column">
                <label for="days">Päiväsakon rahamäärä (à):</label>
                <input
                    type="number"
                    bind:value={fine.amount}
                    min="0"
                    placeholder="Amount (€)"
                />
            </div>
            <div class="fine-column">
                <label for="days">Osasuoritus:</label>
                <input
                    type="number"
                    bind:value={fine.paid}
                    min="0"
                    placeholder="Paid (€)"
                />
            </div>

            <button class="rmbtn" on:click={() => removeDayFine(index)}
                >X</button
            >
        </div>
        <p class="fine-verdict">{fine.fver}</p>
    {/each}

    <button class="addbtn" on:click={addDayFine}>Lisää päiväsakko</button>

    <div class="divider"></div>

    {#each threatFines as fine, index}
        <div class="fine-entry">
            <div class="fine-column">
                <label for="days">Uhkasakon euromäärä (esim. 600€):</label>
                <input
                    type="number"
                    bind:value={fine.quantity}
                    min="0"
                    placeholder="Total"
                />
            </div>
            <div class="fine-column">
                <label for="">Uhkasakon osasuoritus</label>

                <input
                    type="number"
                    bind:value={fine.paid}
                    min="0"
                    placeholder="Paid"
                />
            </div>

            <button class="rmbtn" on:click={() => removeThreatFine(index)}
                >X</button
            >
        </div>
        <p class="fine-verdict">{fine.fver}</p>
    {/each}

    <p>Yhteensä maksamatta {ttotal}€</p>
    <button class="addbtn" on:click={addThreatFine}>Lisää uhkasakko</button>


    <h3>Vankeutta päiväsakoista: {pverdict}</h3>
    <h3>Vankeutta uhkasakoista: {tverdict}</h3>
    <h3>Yhteensä vankeutta: {totalver} päivää.</h3>
</main>

<style>
    .divider {
        width: 100%;
        height: 4px;
        background-color: rgb(34, 97, 32);
    }
    .person {
        font-weight: bold;
        font-size: 1.2em;
        margin-bottom: 2rem;
    }
    .fine-verdict {
        margin-top: -1em;
    }
    main {
        padding: 1em;
        max-width: 40rem;
        margin: 0 auto;
    }
    div {
        margin-bottom: 1em;
    }
    label {
        display: block;
        margin-bottom: 0.5em;
    }
    input {
        width: 100%;
        padding: 0.5em;
    }
    .addbtn {
        margin-bottom: 2rem;
        width: 100%;
        padding: 0.5em;
        background-color: #4caf50;
        color: white;
        border: none;
        cursor: pointer;
    }
    button:hover {
        background-color: #45a049;
    }

    .fine-entry {
        display: flex;
        align-items: center;
        justify-content: space-between;
        margin-bottom: 1em;
    }

    .rmbtn {
        height: 2em;
        width: 2em;
        background-color: #f44336;
        color: white;
        border: none;
        cursor: pointer;
        margin-left: 1em;
    }
</style>
