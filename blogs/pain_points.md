[HOME](../README.md)

---

# Pain Points only make you stronger! 

Holy Hell Batman!! I thought it was smooth coasting prior to this last phase, boy was I wrong!!  The learning curve for this project was intense.  What I thought would take a day to connect front/backend with Redux/Thunk ended up taking me a few days.  (*I know, I should be better by now!*)  But I guess in the end, it is these painful times that make you stronger.  It was like all of a sudden, it just CLICKED!

## Issues from the beginning...

It was my first time since the CLI project that I was using API calls to get my data, so I pushed myself to try something different with this project.  I created an initial setup of migrations and associations, based on my planned user stories.  However, I quickly realized that my plans would not be obtainable within a weeks time.  I guess that's the start of becoming a developer, learning how long tasks will take.  I had to constintly reasses my project ans my expectations of completion. 

After obtaining all my data that would be relavant for my project and my associations were all linked, it was time to start having fun, *or so I thought*

## Connecting it all ~ that includes Mentally too!

So the last week of lectures, WOW that was just a boat load of info! *Redux / Authorizations / Thunk* all while unfortunately a touch of Senioritize set in (we were almost done!!!)  Going through the lectures, it seemed to make sense - however, labs only touched on parts and not the whole sha-bang.  I got my react page setup, I know I was going to need state, so I did my `mapStateToProps` and passed in my teams.  Then came the start of the confusion ... **dispatch**!  I'm not sure why mapStateToProps made sense, however writing out the function `mapDispatchToProps` and calling the dispatch action within, confused me.  It wasn't until I passed in my Actions as objects that it made sense to me.  I ended up writing all my functions in my ActionCreator files, and returning the payload (data) that I want my state to change.  It clicked then for me.  Honestly, I'd probably get confused again writing out dispatch as a function, thanks God for ability to get the job done in multiple ways!

## Challenge I faced...

Creating my lineup proved to be a bit of a challenge for me.  How do I handle the information and store it the best way possible?  See, sometimes downfall of solo projects is that your the only one making decisions, so you can keep telling yourself the next idea is better and you should spend the time to improve.  Yes, sometimes that's the case, the time is worth it ~ other its just a time suck! (*Another one of those learning pain points!*) 

I had issues on how to go about creating my lineup.  Knowing that my lineup would consist of many players, I had to create join table of lineup_players, which then took in additional fields of position and a boolean for the designated hitter.

My form took in 9 different positions, so I had to loop through the results of those input fields and create the objects I wanted to send to my dispatch action to POST to my fetch request.

```javascript
const onSubmit = (event) => { 
        const order = []
        const inputs = document.forms["lineup-form"].getElementsByTagName("input")
        
            for (let i = 0; i < inputs.length; i++){
                order.push({position: i+1, mlb_player_id: inputs[i].name, dh:(inputs[i].id === "DH" ? true : false)})
            }        
          props.newLineup({mlb_team_id: props.team.id, season: 2020}, order)
     }
```

Passing through my Authorization token, since this was specific to my user.  (They are forced to login to access this page)
I had to create a nested fetch request.  I had a Lineup that needed to be created/saved to get the correct id to post my players to. Once I obtained that, I passed that to my next fetch request to then loop through the roster array I passed through to my action.  

```javascript
export const newLineup = (lineupData, lineupRoster) => {
    return (dispatch) => {
        fetch(`${API}/lineups`, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': localStorage.token
            },
            body: JSON.stringify(lineupData)
        })
        .then(response => response.json())
        .then(lineup => {
            lineup.roster = []
            lineupRoster.forEach(player => {
                fetch(`${API}/lineups/${lineup.id}/players`, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': localStorage.token
                    },
                    body: JSON.stringify(player)
                })
                .then(response => response.json())
                .then(player => {
                    lineup.roster.push(player)
                })
            })
            
            return lineup
        })
        .then(lineup =>  dispatch({ type: 'NEW_LINEUP', payload: lineup }))
    }
}
```

I Created an empty array within my returned lineup object, I used this to push in the values from my response of my nested fetch to post the player to the lineup.  By doing this, it enabled the lineup object to now include a new array of the players associated.  Once it is complete, it will rerender the page, and show the User's lineups.

## Take aways

Once that button clicked on how it all relates and flows, I felt that my project took a major shift in a better direction.  I ended up enjoying and seeing the wonderful benifits of Redux/Thunk. Knowing that the potential for working on large scale projects in the future could utilize this software, I am proud of myself for getting passed that learning curve and frustrations.