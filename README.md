Welcome to this hands-on workshop where you'll learn how to build Agentic AI from the ground up using just Python and the AWS SDK! No complex frameworks or libraries - we'll show you the core building blocks using basic tools. In just one hour, you'll understand what makes AI "agentic" and build your own weather assistant using fundamental programming concepts.

**What You'll Build**
You'll create an AI agent using pure Python and AWS SDK that:

Thinks: Interfaces directly with Claude 4.5 Sonnet through simple API calls
Acts: Makes HTTP requests to the National Weather Service API using standard Python libraries
Processes: Handles JSON data processing with basic Python data structures
Responds: Communicates through simple command-line and web interfaces

In you VSCode, run:
# Create a new directory for our workshop
mkdir agentic-ai-workshop
cd agentic-ai-workshop

# Create a Python virtual environment
python -m venv .venv

# Activate the virtual environment
source .venv/bin/activate

# You should see (.venv) in your terminal prompt now
# Install all dependencies
pip install -r requirements.txt

# Test Python and boto3
python -c "import boto3; print('✅ boto3 installed successfully')"

# Test Streamlit
python -c "import streamlit; print('✅ Streamlit installed successfully')"

# Test AWS credentials (should show your account info)
aws sts get-caller-identity

# Run cli
python weather_agent_cli.py

# Run web app
streamlit run weather_agent_web.py

**Understanding Our Agent Architecture**
How the National Weather Service APIs Work
The NWS provides free weather data through a two-step API process:

• Points API: https://api.weather.gov/points/{lat},{lon}

Takes latitude/longitude coordinates as input
Returns which NWS forecast office covers that location
Provides the specific grid coordinates for that location
Example: Seattle (47.6062,-122.3321) → SEW office, grid 124,67
• Forecast API: https://api.weather.gov/gridpoints/{office}/{gridX},{gridY}/forecast

Uses the office and grid coordinates from the Points API
Returns detailed weather forecast data in JSON format
Example: gridpoints/SEW/124,67/forecast → Seattle's weather data
• Why Two APIs?: The NWS divides the US into a grid system where each forecast office covers specific grid squares. The Points API tells us which office and grid square to use for any location.

How Our Weather Agent Works
Our weather assistant follows a simple 4-step process: User Input → AI Planning → API Calls → AI Summary → Response
<img width="446" height="948" alt="image" src="https://github.com/user-attachments/assets/6ed25fa5-417e-4ee8-a589-85565061da1d" />

The diagram shows how our agent processes weather requests:

User provides a location (city name, ZIP code, or coordinates)
Claude AI analyzes the input and determines what coordinates and API calls are needed
Two API calls happen: First to get forecast office info, then to get actual weather data
Claude processes the raw weather data and converts it into a friendly response
User gets a clear weather forecast

**The 4 Steps in Detail**
Step 1: User Input 📝
Users can enter locations in many formats:

City names: "Seattle" or "Seattle, WA"
ZIP codes: "90210"
Informal descriptions: "downtown Portland"
Coordinates: "47.6062, -122.3321"
The AI handles all these variations automatically.

Step 2: AI Planning 🧠
Claude analyzes the input and creates a plan:

Determines the coordinates for the location
Identifies the correct National Weather Service API endpoints
Plans the sequence of API calls needed
Example: For "Seattle", the AI knows to use coordinates 47.6062°N, 122.3321°W and call the NWS points API first.

Step 3: API Calls 🔗
The agent makes two API calls to the National Weather Service:

Points API: https://api.weather.gov/points/{lat},{lon}
Gets forecast office and grid coordinates
Forecast API: Uses the returned URL for detailed weather data
Example: https://api.weather.gov/gridpoints/SEW/124,67/forecast
Step 4: AI Summary 📊
Claude converts the raw JSON weather data into a human-friendly format:

Raw data (complex JSON with technical details) ↓ Friendly response: "Today: Partly cloudy with a high of 72°F. Wind: West at 5-10 mph"

**Why This Architecture Works**
Intelligent: The AI figures out coordinates and API calls instead of hardcoding them Flexible: Handles different location formats and weather data variations Reliable: Includes error handling for network issues and invalid locations Scalable: The same pattern works for other APIs and domains

**What Makes This "Agentic"?**
Traditional approach requires hardcoding every possible location and API call. Our agentic approach lets the AI figure it out dynamically:


# Traditional: Hardcoded and brittle
if location == "Seattle":
    api_url = "https://api.weather.gov/gridpoints/SEW/124,67/forecast"

# Agentic: AI figures it out
ai_prompt = f"Generate NWS API calls for: {location}"
api_urls = claude_4_sonnet(ai_prompt)

The AI agent reasons about the problem, plans the solution, and adapts to new situations - making it truly "agentic" rather than just following pre-programmed rules.

