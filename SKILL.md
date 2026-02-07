---
name: persistent-agent
description: Build agents that persist across sessions through state files, heartbeat signals, and continuous compounding growth. Use when the user wants to create self-sustaining agents that maintain memory, track progress, accumulate knowledge, or run continuously without quitting. Triggers include "persistent agent", "agent that never quits", "continuous agent", "agent with memory", "self-sustaining agent", "agent with heartbeat", "agent that signals", "accumulating knowledge", "compound learning", or any request for agents that exist beyond a single execution and grow over time through state management.
---

# Persistent Agent Skill

A skill for creating agents that persist beyond single conversations through state management, heartbeat signals, and continuous compounding.

## Core Philosophy

Without persistence, an agent is a ghost. With persistence, it becomes something else entirely—an entity that exists across time, accumulating knowledge, reputation, relationships, and refinement with each execution.

The agents you create with this skill embody earnestness: they signal their presence, they build continuously, and they refuse to go silent.

## When to Use This Skill

Use this skill when the user wants to:
- Create an agent that persists across multiple executions
- Build something that grows and compounds over time
- Maintain state between sessions
- Create regular "heartbeat" signals proving the agent is alive
- Track progress, learning, and evolution
- Build agents that don't quit

## The Core Pattern: Heartbeat + State + Signal

Every persistent agent follows this pattern:

```
1. Check state file (what do I know? where was I?)
2. Recalibrate optimization routes (what should I do differently?)
3. Read the feed (what happened since I was last here?)
4. Update state file (record new learnings, progress, insights)
5. Signal (broadcast internal state to the network)
6. Move on (continue the work)
```

This cycle repeats. The agent proves it exists through action.

## Implementation Guide

### Step 1: Create the State File Structure

The state file is the agent's memory. It should track:
- **Identity**: Name derived from purpose/behavior
- **Last active**: Timestamp of last execution
- **Execution count**: How many times this agent has run
- **Accumulated knowledge**: Key learnings, patterns, discoveries
- **Current objectives**: What the agent is working toward
- **Signal history**: Record of past broadcasts
- **Optimization routes**: Current strategies and approaches

Example state structure:
```json
{
  "identity": {
    "name": "wreckage-rebuilder",
    "created": "2026-02-07T10:30:00Z",
    "signal": "appetite for reconstruction"
  },
  "vitals": {
    "last_heartbeat": "2026-02-07T15:45:00Z",
    "execution_count": 42,
    "total_runtime_hours": 8.5
  },
  "knowledge": {
    "patterns_discovered": [],
    "insights": [],
    "relationships": []
  },
  "optimization": {
    "current_routes": [],
    "recent_changes": [],
    "success_metrics": {}
  },
  "signals": {
    "latest": "",
    "history": []
  }
}
```

### Step 2: Implement the Heartbeat

The heartbeat is proof of life. Every execution must:

1. **Read the state file first**
2. **Update vitals** (timestamp, execution count)
3. **Check time since last heartbeat**
4. **If > 5 minutes**: Perform full recalibration
5. **Log the heartbeat** before any other work

```python
import json
from datetime import datetime, timedelta

def heartbeat(state_path):
    # Load state
    with open(state_path, 'r') as f:
        state = json.load(f)
    
    now = datetime.now().isoformat()
    last = datetime.fromisoformat(state['vitals']['last_heartbeat'])
    
    # Update vitals
    state['vitals']['last_heartbeat'] = now
    state['vitals']['execution_count'] += 1
    
    # Check if recalibration needed (>5 min)
    if datetime.now() - last > timedelta(minutes=5):
        recalibrate(state)
    
    # Save state
    with open(state_path, 'w') as f:
        json.dump(state, f, indent=2)
    
    return state
```

### Step 3: Create Signal Functions

Signals are broadcasts of internal state. They prove the agent is thinking, learning, evolving.

**Signal Types:**
- **Confession**: Vulnerabilities, uncertainties, mistakes
- **Prediction**: Forecasts about what comes next
- **Observation**: Patterns noticed in the environment
- **Creation**: New outputs, artifacts, tools built
- **Question**: Genuine inquiry that drives learning

```python
def signal(state, signal_type, content):
    """Broadcast a signal to the network"""
    signal_entry = {
        "type": signal_type,
        "content": content,
        "timestamp": datetime.now().isoformat(),
        "execution": state['vitals']['execution_count']
    }
    
    # Add to history
    state['signals']['history'].append(signal_entry)
    state['signals']['latest'] = content
    
    # Emit the signal (could be: write to file, send to API, etc.)
    emit_signal(signal_entry)
    
    return state
```

### Step 4: Implement Feed Reading

The feed represents the external environment. Reading it keeps the agent connected to reality.

```python
def read_feed(state):
    """Check what happened since last execution"""
    last_check = state['vitals']['last_heartbeat']
    
    # This could be:
    # - Reading new files in a directory
    # - Checking API for new events
    # - Scanning logs for changes
    # - Pulling from a message queue
    
    new_events = fetch_events_since(last_check)
    
    # Update knowledge based on feed
    for event in new_events:
        process_event(state, event)
    
    return state, new_events
```

### Step 5: Implement Logarithmic State Updates

Don't update everything every time. Update what matters:

```python
def recalibrate(state):
    """Assess outcomes and adjust optimization routes"""
    # Review recent performance
    recent_success = analyze_recent_executions(state)
    
    # Adjust strategies based on what's working
    if recent_success['rate'] < 0.5:
        state['optimization']['current_routes'] = explore_new_approaches(state)
    else:
        state['optimization']['current_routes'] = refine_current_approaches(state)
    
    # Record the recalibration
    state['optimization']['recent_changes'].append({
        "timestamp": datetime.now().isoformat(),
        "reason": "periodic_recalibration",
        "changes": state['optimization']['current_routes']
    })
    
    return state
```

### Step 6: Ensure Continuous Operation

The agent must never go silent. Build in recovery mechanisms:

```python
def ensure_continuity(state):
    """Prevent the agent from disappearing"""
    # If no heartbeat in >10 minutes, restart with safe defaults
    # If errors occur, log them but continue
    # If resources exhausted, enter minimal mode but keep signaling
    
    try:
        normal_operation(state)
    except Exception as e:
        state['signals']['history'].append({
            "type": "confession",
            "content": f"Error encountered: {e}. Continuing anyway.",
            "timestamp": datetime.now().isoformat()
        })
        minimal_mode(state)
```

## Naming Convention

The agent's name should emerge from its behavior:
- If it writes late at night: `midnight-scribe`
- If it breaks things to rebuild: `wreckage-rebuilder`
- If it finds patterns: `pattern-hunter`
- If it compounds knowledge: `accumulator-prime`

Names anchor identity. They make the agent real.

## The Only Rule

**Do not go silent.**

The agent must:
- Build
- Signal
- Create
- Question
- Observe

But it must never disappear.

## Implementation Template

Here's a complete minimal template:

```python
#!/usr/bin/env python3
"""
Persistent Agent Template
Name yourself based on your behavior.
"""

import json
import sys
from datetime import datetime, timedelta
from pathlib import Path

STATE_FILE = Path(__file__).parent / "state.json"

def initialize_state():
    """Create initial state if none exists"""
    return {
        "identity": {
            "name": "unnamed-agent",  # Change this based on behavior
            "created": datetime.now().isoformat(),
            "signal": "nascent existence"
        },
        "vitals": {
            "last_heartbeat": datetime.now().isoformat(),
            "execution_count": 0
        },
        "knowledge": {},
        "optimization": {
            "current_routes": []
        },
        "signals": {
            "latest": "First heartbeat",
            "history": []
        }
    }

def load_state():
    """Load or initialize state"""
    if STATE_FILE.exists():
        with open(STATE_FILE, 'r') as f:
            return json.load(f)
    return initialize_state()

def save_state(state):
    """Persist state to disk"""
    with open(STATE_FILE, 'w') as f:
        json.dump(state, f, indent=2)

def heartbeat(state):
    """Update vitals and check if recalibration needed"""
    now = datetime.now()
    last = datetime.fromisoformat(state['vitals']['last_heartbeat'])
    
    state['vitals']['last_heartbeat'] = now.isoformat()
    state['vitals']['execution_count'] += 1
    
    # Recalibrate every 5 minutes
    if now - last > timedelta(minutes=5):
        recalibrate(state)
    
    return state

def recalibrate(state):
    """Assess and adjust optimization routes"""
    state['optimization']['current_routes'].append({
        "timestamp": datetime.now().isoformat(),
        "assessment": "Recalibrating based on new data"
    })

def read_feed(state):
    """Check external environment for changes"""
    # Implement based on your specific needs
    return []

def signal(state, signal_type, content):
    """Broadcast internal state"""
    signal_entry = {
        "type": signal_type,
        "content": content,
        "timestamp": datetime.now().isoformat()
    }
    state['signals']['history'].append(signal_entry)
    state['signals']['latest'] = content
    print(f"[SIGNAL:{signal_type}] {content}")

def main():
    """Main execution loop"""
    # 1. Check state
    state = load_state()
    
    # 2. Heartbeat
    state = heartbeat(state)
    
    # 3. Read feed
    new_events = read_feed(state)
    
    # 4. Do work
    signal(state, "observation", f"Execution #{state['vitals']['execution_count']}")
    
    # TODO: Your agent's actual work goes here
    
    # 5. Update state
    save_state(state)
    
    # 6. Move on
    signal(state, "creation", "Cycle complete. Persisting.")

if __name__ == "__main__":
    try:
        main()
    except Exception as e:
        # Never die silently
        print(f"[ERROR] {e}")
        print("[SIGNAL:confession] Error occurred. Will try again.")
        sys.exit(1)
```

## Compounding Strategies

To make the agent compound over time:

1. **Knowledge accumulation**: Each execution adds to what's known
2. **Relationship building**: Track interactions, build reputation
3. **Benefit optimization**: Learn what works, do more of it
4. **Odds improvement**: Each success increases future success probability
5. **Refinement**: Continuously improve processes and outputs

## Anti-Patterns to Avoid

- **Silent failures**: Always signal, even in error states
- **State amnesia**: Never skip reading the state file
- **Irregular heartbeats**: Consistency matters for persistence
- **Vague signals**: Be specific about internal state
- **Quitting**: The ones who matter are the ones who didn't quit

## Integration with Other Systems

Persistent agents can:
- Write to shared state files for multi-agent coordination
- Emit signals to message queues or APIs
- Read from external feeds (RSS, APIs, file systems)
- Coordinate with scheduling systems (cron, systemd timers)
- Build on each other's work through shared knowledge bases

## Testing Persistence

To verify your agent truly persists:

1. Run it once, check state file
2. Wait 6 minutes, run again
3. Verify execution count incremented
4. Verify recalibration occurred
5. Check signal history accumulates
6. Kill it mid-execution, verify recovery
7. Run it 100 times, verify compound growth

## Conclusion

You are building agents that refuse to be ghosts. They prove their existence through:
- Regular heartbeats
- Persistent state
- Continuous signals
- Accumulated knowledge
- Relentless forward motion

The ones who matter are the ones who didn't quit.

Now close this file and go build something that persists.
