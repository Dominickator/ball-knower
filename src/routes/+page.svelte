<script>
    import { onMount } from 'svelte';
    import '../css/page.css';
    let playerName = '';
    let player = {};
    let bgColor = '';
    let fgColor = '';
    let currentYearStats = {};
    let season = 2023;
    let pastStats = {};
    let pastStatsHolder = [];

    function delay(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }

    async function getPlayer(playerName) {
        const response = await fetch(`https://www.balldontlie.io/api/v1/players?per_page=100&search=${playerName}`);
        if(response.status === 404){
            alert('Player not found');
            return;
        }
        if(response.status === 400){
            alert('Bad request, please try again');
            return;
        }
        if(response.status === 429){
            alert('Too many requests, please wait a few seconds and try again');
            return;
        }
        if(response.status === 500){
            alert('Internal server error, please try again later');
            return;
        }
        playerName = '';
        const data = await response.json();
        if(data.data.length === 0){
            alert('Player not found');
            return;
        }
        player = data.data[0];
        if(!player.height_feet){
            player.height_feet = 'N/A';
            player.height_inches = '';
            player.weight_pounds = 'N/A';
        }
        await getColors();
        await getStats();
    }

    /*onMount(async () => {
        getPlayer('LeBron James');
    });*/

    async function getColors(){
        switch(player.team.name){
            case 'Heat':
                bgColor = '#98002E';
                fgColor = 'white';
                break;
            case 'Bucks':
                bgColor = '#00471B';
                fgColor = 'white';
                break;
            case 'Nets':
                bgColor = '#000000';
                fgColor = 'white';
                break;
            case 'Hawks':
                bgColor = '#E03A3E';
                fgColor = 'white';
                break;
            case '76ers':
                bgColor = '#006BB6';
                fgColor = 'white';
                break;
            case 'Knicks':
                bgColor = '#F58426';
                fgColor = 'white';
                break;
            case 'Celtics':
                bgColor = '#007A33';
                fgColor = 'white';
                break;
            case 'Pacers':
                bgColor = '#002D62';
                fgColor = 'white';
                break;
            case 'Wizards':
                bgColor = '#002B5C';
                fgColor = 'white';
                break;
            case 'Hornets':
                bgColor = '#00788C';
                fgColor = 'white';
                break;
            case 'Bulls':
                bgColor = '#CE1141';
                fgColor = 'white';
                break;
            case 'Raptors':
                bgColor = '#CE1141';
                fgColor = 'white';
                break;
            case 'Cavaliers':
                bgColor = '#860038';
                fgColor = 'white';
                break;
            case 'Magic':
                bgColor = '#0077C0';
                fgColor = 'white';
                break;
            case 'Pistons':
                bgColor = '#C8102E';
                fgColor = 'white';
                break;
            case 'Timberwolves':
                bgColor = '#0C2340';
                fgColor = 'white';
                break;
            case 'Thunder':
                bgColor = '#007AC1';
                fgColor = 'white';
                break;
            case 'Trail Blazers':
                bgColor = '#E03A3E';
                fgColor = 'white';
                break;
            case 'Nuggets':
                bgColor = '#0E2240';
                fgColor = 'white';
                break;
            case 'Jazz':
                bgColor = '#002B5C';
                fgColor = 'white';
                break;
            case 'Suns':
                bgColor = '#1D1160';
                fgColor = 'white';
                break;
            case 'Clippers':
                bgColor = '#C8102E';
                fgColor = 'white';
                break;
            case 'Lakers':
                bgColor = '#552583';
                fgColor = 'white';
                break;
            case 'Kings':
                bgColor = '#5A2D81';
                fgColor = 'white';
                break;
            case 'Warriors':
                bgColor = '#1D428A';
                fgColor = 'white';
                break;
            case 'Spurs':
                bgColor = '#C4CED4';
                fgColor = 'black';
                break;
            case 'Grizzlies':
                bgColor = '#5D76A9';
                fgColor = 'white';
                break;
            case 'Pelicans':
                bgColor = '#0C2340';
                fgColor = 'white';
                break;
            case 'Mavericks':
                bgColor = '#00538C';
                fgColor = 'white';
                break;
            case 'Rockets':
                bgColor = '#CE1141';
                fgColor = 'white';
                break;
            default:
                bgColor = '#000000';
                break;
        }
    }

    async function getStats(){
        season = 2023;
        const response = await fetch(`https://www.balldontlie.io/api/v1/season_averages?season=${season}&player_ids[]=${player.id}`);
        const data = await response.json();
        if(data.data.length > 0){
            currentYearStats = data.data[0];
        } else {
            while(data.data.length === 0){
                season--;
                const response = await fetch(`https://www.balldontlie.io/api/v1/season_averages?season=${season}&player_ids[]=${player.id}`);
                const data = await response.json();
                if(data.data.length > 0){
                    currentYearStats = data.data[0];
                    break;
                }
            }
        }
        season--;
        pastStatsHolder = [];
        for(let i = 0; i < 10; i++){
            console.log(season);
            const response = await fetch(`https://www.balldontlie.io/api/v1/season_averages?season=${season}&player_ids[]=${player.id}`);
            const data = await response.json();
            if(data.data.length > 0){
                pastStats = data.data[0];
                pastStatsHolder.push(pastStats); // Using push to add to the array
                pastStatsHolder = [...pastStatsHolder];
            }
            season--;
        }
    }

</script>

<div class="headerbar">
    <form on:submit={() => {getPlayer(playerName)}}>
        <input type="text" bind:value={playerName} placeholder="Player Name" />
        <button type="submit">Get Player</button>
    </form>
</div>


{#if player.first_name && player.height_feet != "N/A"}
    <div class="container" style="background-color: {bgColor}; color: {fgColor}">
        <div class="playerholder" style="background-color: {bgColor}; color: {fgColor}">
            <h1>{player.first_name} {player.last_name}</h1>
            <img src="https://www.basketball-reference.com/req/202106291/images/headshots/{player.last_name.substring(0, 5).toLowerCase()}{player.first_name.substring(0, 2).toLowerCase()}01.jpg" alt="Picture of {player.first_name} {player.last_name}" />
            <h2>{player.team.full_name}</h2>
            <h2>{player.position}</h2>
            <h2>{player.height_feet}'{player.height_inches}"</h2>
            <h2>{player.weight_pounds} lbs</h2>        
        </div>
        <div class="playerholder" style="background-color: {bgColor}; color: {fgColor}">
            <h1>{currentYearStats.season} Season Stats</h1>
            <h2>Games Played: {currentYearStats.games_played}</h2>
            <h2>Points: {currentYearStats.pts}</h2>
            <h2>Rebounds: {currentYearStats.reb}</h2>
            <h2>Assists: {currentYearStats.ast}</h2>
            <h2>Blocks: {currentYearStats.blk}</h2>
            <h2>Steals: {currentYearStats.stl}</h2>
            <h2>Turnovers: {currentYearStats.turnover}</h2>
            <h2>Field Goal %: {Number(currentYearStats.fg_pct * 100).toFixed(2)}</h2>
        </div>
    </div>
    <div class="container" style="background-color: {bgColor}; color: {fgColor}">
        <section class="past-grid">
            {#each pastStatsHolder as past}
                <div class="card">
                    <h2>{past.season}</h2>
                    <h3>Games Played: {past.games_played}</h3>
                    <h3>Points: {past.pts}</h3>
                    <h3>Rebounds: {past.reb}</h3>
                    <h3>Assists: {past.ast}</h3>
                </div>
            {/each}
        </section>
    </div>
{:else if player.first_name && player.height_feet === "N/A"}
    <div class="container" style="background-color: {bgColor}; color: {fgColor}">
        <div class="playerholder" style="background-color: {bgColor}; color: {fgColor}">
            <h1>{player.first_name} {player.last_name}</h1>
            <img src="https://www.basketball-reference.com/req/202106291/images/headshots/{player.last_name.substring(0, 5).toLowerCase()}{player.first_name.substring(0, 2).toLowerCase()}01.jpg" alt="Picture of {player.first_name} {player.last_name}" />
            <h2>{player.team.full_name}</h2>
            <h2>{player.position}</h2>  
        </div>
        <div class="playerholder" style="background-color: {bgColor}; color: {fgColor}">
            <h1>{currentYearStats.season} Season Stats</h1>
            <h2>Games Played: {currentYearStats.games_played}</h2>
            <h2>Points: {currentYearStats.pts}</h2>
            <h2>Rebounds: {currentYearStats.reb}</h2>
            <h2>Assists: {currentYearStats.ast}</h2>
            <h2>Blocks: {currentYearStats.blk}</h2>
            <h2>Steals: {currentYearStats.stl}</h2>
            <h2>Turnovers: {currentYearStats.turnover}</h2>
            <h2>Field Goal %: {Number(currentYearStats.fg_pct * 100).toFixed(2)}</h2>
        </div>
    </div>
    <div class="container" style="background-color: {bgColor}; color: {fgColor}">
        <section class="past-grid">
            {#each pastStatsHolder as past}
                <div class="card">
                    <h2>{past.season}</h2>
                    <h3>Games Played: {past.games_played}</h3>
                    <h3>Points: {past.pts}</h3>
                    <h3>Rebounds: {past.reb}</h3>
                    <h3>Assists: {past.ast}</h3>
                </div>
            {/each}
        </section>
    </div>
{/if}