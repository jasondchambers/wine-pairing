# wine-pairing

A Claude Code skill for wine pairing recommendations.

A Claude Code skill that recommends wine pairings from your personal wine cellar. Tell Claude what dish you're cooking, and it will suggest wines from your collection based on classic pairing principles and drinking windows.

## Features

- **Smart Pairing**: Uses wine pairing principles to match wines to your dish
- **Two Recommendation Lists**:
  - **Drink Soon**: Wines past their drinking window (sorted by score) - helps you consume wines before they decline further
  - **Best Available**: Highest-rated wines regardless of drinking window - for special occasions
- **Cellar Integration**: Queries your actual wine inventory via [winebuddy](https://github.com/jasondchambers/winebuddy)

## Prerequisites

1. **CellarTracker Account**: Export your wine cellar from [CellarTracker](https://www.cellartracker.com) as CSV
2. **Git**: For cloning the winebuddy repository
3. **uv**: Python package manager ([install uv](https://docs.astral.sh/uv/getting-started/installation/))

## Getting Started

### 1. Export Your Cellar from CellarTracker

1. Log in to [CellarTracker](https://www.cellartracker.com)
2. Navigate to your cellar
3. Export as CSV with these columns:
   - Color, Category, Size, Currency, Value, Price, TotalQuantity
   - Quantity, Pending, Vintage, Wine, Locale, Producer, Varietal
   - Country, Region, SubRegion, BeginConsume, EndConsume, PScore, CScore

### 2. Place Your Cellar Data

Save your exported CSV file to:
```
~/.winebuddy/cellar.csv
```

The skill will automatically clone the winebuddy tool to `~/.winebuddy/` on first run if it doesn't exist.

## Installation

### Claude Code

#### Option A: Global Installation (Recommended)

Install the skill globally so it's available in all your projects:

```bash
git clone https://github.com/jasondchambers/wine-pairing ~/.claude/skills/wine-pairing
```

Restart Claude Code to pick up the new skill.

#### Option B: Project-Specific Installation

Install the skill for a specific project only:

```bash
cd /path/to/your/project
git clone https://github.com/jasondchambers/wine-pairing .claude/skills/wine-pairing
```

Restart Claude Code to pick up the new skill.

### Claude Desktop

Claude Desktop does not currently support skills in the same way as Claude Code. However, you can achieve similar functionality by:

1. **Using the winebuddy CLI directly** in your prompts:
   ```
   Run this command and recommend wine pairings for grilled salmon:
   cd ~/.winebuddy && uvx --from . winebuddy query --in-stock --format json
   ```

2. **Creating an MCP Server** (advanced): Wrap the winebuddy CLI in an MCP server for tighter integration with Claude Desktop.

## Usage

Once installed, simply ask Claude about wine pairings:

```
What wine should I pair with grilled ribeye steak?
```

```
I'm making seafood pasta tonight, what wine do you recommend?
```

```
/wine-pairing roasted chicken with herbs
```

### Example Output

```
## Wine Pairing Recommendations for Grilled Ribeye Steak

### Drink Soon (Past Drinking Window)
These wines have passed their optimal drinking window and should be enjoyed soon:

1. **14 Hands Cabernet Sauvignon The Reserve** (2016)
   - Producer: 14 Hands
   - Varietal: Cabernet Sauvignon | Region: Washington
   - Score: N/A | Value: $29.01 | Drinking Window: 2019-2023
   - Why it pairs: Classic Cabernet is the quintessential steak pairing...

### Best Available (For Special Occasions)
Your highest-rated wines that pair beautifully with this dish:

1. **Caymus Cabernet Sauvignon** (2020)
   - Producer: Caymus
   - Varietal: Cabernet Sauvignon | Region: California, Napa Valley
   - Score: N/A | Value: $163.40 | Drinking Window: 2024-2033
   - Why it pairs: Napa Cabernet is legendary with steak...
```

## How It Works

1. **Setup**: On first run, clones [winebuddy](https://github.com/jasondchambers/winebuddy) to `~/.winebuddy/`
2. **Query**: Runs `winebuddy query --in-stock --format json --sort score --desc` to get your cellar inventory
3. **Analyze**: Applies wine pairing principles to match wines to your dish
4. **Recommend**: Generates two lists:
   - Wines past their drinking window (`end_consume < current year`)
   - Best available wines by score/value

## Troubleshooting

### "Cellar data not found"

Make sure your `cellar.csv` is at `~/.winebuddy/cellar.csv`. The file must be exported from CellarTracker with the required columns.

### Skill not recognized

Restart Claude Code after installing the skill. Skills are loaded at startup.

### No wines in recommendations

- Check that your cellar has wines in stock (`quantity > 0`)
- Verify your cellar.csv was exported correctly from CellarTracker

## License

MIT

## Related

- [winebuddy](https://github.com/jasondchambers/winebuddy) - The CLI tool that powers this skill
- [CellarTracker](https://www.cellartracker.com) - Wine cellar management service
