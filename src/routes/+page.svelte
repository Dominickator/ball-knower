<script>
    import '../css/page.css';
    let playerName = '';
    let player = {};
    let bgColor = '';
    let fgColor = '';
    let currentYearStats = {};

    async function getPlayer(playerName) {
        const response = await fetch(`https://www.balldontlie.io/api/v1/players?per_page=100&search=${playerName}`);
        playerName = '';
        const data = await response.json();
        player = data.data[0];
        getColors();
        getStats();
    }

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
        const response = await fetch(`https://www.balldontlie.io/api/v1/season_averages?season=2023&player_ids[]=${player.id}`);
        const data = await response.json();
        currentYearStats = data.data[0];
    }

</script>

<div class="headerbar">
    <form on:submit={() => {getPlayer(playerName)}}>
        <input type="text" bind:value={playerName} placeholder="Player Name" />
        <button type="submit">Get Player</button>
    </form>
</div>


{#if player.first_name}
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
            <h2>Field Goal %: {(currentYearStats.fg_pct * 100).toFixed(2)}</h2>
        </div>
    </div>
{/if}
