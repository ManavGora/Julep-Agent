# 🍽️ Foodie Tour with Julep

This project builds a smart workflow using [Julep](https://www.julep.ai/) to generate delightful foodie tours in various cities. For each city you provide, the workflow:

1. **Checks today's weather**
2. **Recommends indoor/outdoor dining**
3. **Picks 3 iconic dishes for the city**
4. **Finds top-rated restaurants for those dishes**
5. **Creates a 1-day foodie tour narrative** including breakfast, lunch, dinner

---

## 🔧 Tech Stack

* **Julep AI**: LLM + tool-based workflow automation
* **OpenWeather API**: Weather lookup
* **Brave Search API**: Restaurant search
* **GPT-4o**: Text generation via Julep Agent

---

## 🛠 Setup Instructions

### 1. Install Julep SDK

```bash
pip install julep
```

### 2. Get API Keys

* Julep: [Sign up](https://dashboard.julep.ai/)
* OpenWeather: [https://openweathermap.org/api](https://openweathermap.org/api)
* Brave Search: [https://search.brave.com/api/](https://search.brave.com/api/)

### 3. Prepare `.env` or Python constants:

```python
OPENWEATHER_API_KEY = "your_openweather_key"
BRAVE_API_KEY = "your_brave_key"
JULEP_API_KEY = "your_julep_key"
```

---

## 📦 Project Structure

```bash
.
├── foodie_tour_workflow.py      # Main logic for creating + executing Julep Task
├── README.md                    # This file
```

---

## 📋 How It Works

### Step 1: Agent Setup

```python
agent = client.agents.create_or_update(
    agent_id=AGENT_UUID,
    name="Foodie Tour Guide",
    about="An AI agent that plans foodie tours",
    model="gpt-4o",
)
```

### Step 2: Define Workflow YAML

See `yaml_str` in `foodie_tour_workflow.py`. The workflow uses:

* `foreach` loop over cities
* `call` step for weather + restaurant tools
* `set` for intermediate values (e.g., temp, dishes)
* `generate` to produce the final foodie narrative

Example logic:

```yaml
- foreach:
    in: $ _.cities
    do:
      - call: get_weather
      - set: city
      - set: weather, temp
      - set: suggestion (indoor/outdoor)
      - set: dishes (city-specific)
      - foreach: dishes → search_restaurants → set top_restaurant
      - generate: breakfast, lunch, dinner plan
```

### Step 3: Execute

```python
execution = client.executions.create(
    task_id=task.id,
    input={"cities": ["Paris", "Tokyo", "New York"]}
)
```

Wait and fetch:

```python
while exec_obj.status != 'succeeded':
    time.sleep(5)
    exec_obj = client.executions.get(execution.id)
```

---

## ✨ Example Output

```
City: Tokyo
Weather: Clear (23°C)
Dining suggestion: outdoor
Iconic dishes: Sushi, Ramen, Okonomiyaki
Breakfast: Enjoy Sushi at Sushiro.
Lunch: Savor Ramen at Ichiran.
Dinner: Indulge in Okonomiyaki at Chibo.
Narrative: Based on the weather, we recommend outdoor dining for your foodie adventure!
```

---

## 🚀 Ideas for Extensions

* Use an LLM tool to dynamically fetch iconic dishes
* Add Google Maps URLs in output
* Handle errors for no restaurant results

---

## 💬 Need Help?

Join the [Julep Discord](https://discord.gg/julep) community!

---

## ⭐ Credits

* Built using [Julep](https://www.julep.ai/)
* Weather from [OpenWeather](https://openweathermap.org)
* Search by [Brave API](https://search.brave.com)
