# 🃏 Bazaar

A strategic card game for 2, 3 or 4 players, played with a standard 52-card deck. Build face-up piles of matching cards, steal from your opponents, and be the first to reach **52 points**.

Play solo against the computer, or online with friends using a room code. The whole game is a single `index.html` file with no build step.

## Features

- Play against computer opponents (2, 3 or 4 players)
- Online multiplayer with room codes (2, 3 or 4 players)
- 4-player team mode: you and your partner against two opponents
- Live chat with quick reactions
- Player names
- Works on phones and desktops

## How to play

**Scoring:** 2–9 = ½ point, 10, J, Q, K = 1 point, Ace = 2 points. Suits do not matter.

**Setup:** every player gets 4 cards per cycle, and 4 cards start face up on the table. A round uses the whole deck (3 cycles with 4 players, 4 with 3 players, 6 with 2 players).

**On your turn you can:**

- **Add to your pile:** place a pair, triplet or quadruplet from your hand, or a single card that matches the top card of your own pile.
- **Take from the table pile:** match its top card with a card from your hand and take the matching cards.
- **Take from an opponent's pile:** match the top card of their pile and take the matching cards.
- **Discard:** put any card on the table pile. You can always discard instead of making a match, and this ends your turn.

**Extra moves:** every valid add or take gives you another move. You can add again, discard, or end your turn without discarding.

**Taking the whole pile:** on your first turn of the first cycle, matching the table's top card lets you take the **entire** table pile (or an opponent's whole unsecured 🔓 pile). Add something to your pile afterwards to **secure** it. Secured piles can only be taken from the top.

**4-player teams:** each team shares one pile. You can see your team's whole pile, and opponents see only its top card.

**Winning:** points from each round add up. The first player or team to reach 52 wins. If several pass 52 in the same round, the highest total wins, and an exact tie is a draw.

## Setting up online play (Firebase, free)

Online rooms and chat store their data in a free Firebase Realtime Database. You only need to do this once.

1. Go to [console.firebase.google.com](https://console.firebase.google.com), sign in, and click **Add project** (you can turn off Analytics).
2. Open **Build → Realtime Database → Create Database**, choose any location, and start in **test mode**.
3. Open the **Rules** tab, replace the contents with the following, and click **Publish**:
   ```json
   {"rules":{"games":{"$code":{".read":true,".write":true}}}}
   ```
4. On the **Data** tab, copy the database URL (for example `https://yourproject-default-rtdb.firebaseio.com`).
5. Open `index.html` in a text editor, find this line near the end of the script, and paste your URL between the quotes:
   ```js
   const DB_URL='PASTE_YOUR_FIREBASE_DATABASE_URL_HERE';
   ```
6. Save the file and commit it to your repository.

Playing against the computer works without any setup.

## Hosting on GitHub Pages

1. Put `index.html` in the root of a public GitHub repository.
2. Go to **Settings → Pages**.
3. Under **Source**, choose **Deploy from a branch**, select `main` and `/ (root)`, and click **Save**.
4. After a minute or two, the game is live at `https://YOUR-USERNAME.github.io/REPOSITORY-NAME/`.

## Playing online

1. One player enters a name, then taps **Create room** and gets a 4-letter code.
2. Friends open the game link, enter a name and the code, and tap **Join room**.
3. When all seats are filled, the creator taps **Start game**.
4. Everyone sees only their own hand. Use the 💬 button for chat.

## How it works

- Single file: HTML, CSS and vanilla JavaScript, with no frameworks or libraries.
- Online play uses the Firebase Realtime Database REST API over HTTPS.
- The game state is saved after every move, and every client polls about every 1.3 seconds.
- Chat messages are posted to the same database and polled every 2 seconds.
- Each browser tab acts as one player. The player ID is kept in `sessionStorage`.

## Limitations

- The database rules are open, so anyone who knows your database URL can read or change game data. This is fine for friends but not for tournaments or public use.
- Hands are stored in the shared game state, so a technical player could look at them.
- If a player closes their tab, they can rejoin only from the same browser tab session. There is no automatic seat takeover.
- Updates arrive with a delay of up to about 2 seconds because of polling.

## Customizing

- **Win target:** change `52` in the scoring code.
- **Colours and card look:** edit the CSS at the top of `index.html`.
- **Computer difficulty:** the `ai()` function picks the highest-value move each turn.

## License

Free to use and modify for personal projects.
