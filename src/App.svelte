<script>
  let occupied = false;
  let movementDetected = false;
  let lastMovement = "No activity yet";
  let monitoredRoom = "Living Room";

  let currentTemperature = 72;
  let targetTemperature = 72;

  let surfaceMode = "Normal";

  let floorLighting = false;
  let automaticLighting = true;

  let roomLights = false;
  let curtainsOpen = false;
  let automationEnabled = true;

  let workoutActive = false;
  let workoutSpeed = 3.0;
  let workoutDistance = 0;

  let selectedScenario = "None";

  let showInfo = false;
  let simulationRunning = false;

  function enterRoom() {
    occupied = true;
    movementDetected = true;
    lastMovement = "Just now";

    if (automationEnabled) {
      roomLights = true;
      curtainsOpen = true;
    }

    if (automaticLighting) {
      floorLighting = true;
    }
  }

function leaveRoom() {
  occupied = false;
  movementDetected = false;
  lastMovement = "Person left room";

  if (automationEnabled) {
    roomLights = false;
    curtainsOpen = false;
  }

  if (automaticLighting) {
    floorLighting = false;
  }
}

  function walkAcrossFloor() {
    occupied = true;
    movementDetected = true;
    lastMovement = "Movement detected just now";

    if (automaticLighting) {
      floorLighting = true;
    }
  }

  function increaseTemperature() {
    if (targetTemperature < 85) {
      targetTemperature++;
    }
  }

  function decreaseTemperature() {
    if (targetTemperature > 55) {
      targetTemperature--;
    }
  }

  function changeSurface(mode) {
    surfaceMode = mode;
  }

  function startWorkout() {
    workoutActive = true;
    surfaceMode = "Exercise";
  }

  function stopWorkout() {
    workoutActive = false;
  }

  function emergencyStop() {
    workoutActive = false;
    workoutSpeed = 0;
  }

  function increaseSpeed() {
    if (workoutSpeed < 8) {
      workoutSpeed = Math.round((workoutSpeed + 0.5) * 10) / 10;
    }
  }

  function decreaseSpeed() {
    if (workoutSpeed > 0.5) {
      workoutSpeed = Math.round((workoutSpeed - 0.5) * 10) / 10;
    }
  }

  function loadScenario(scenario) {
    selectedScenario = scenario;

    if (scenario === "Normal Day") {
      occupied = true;
      movementDetected = true;
      monitoredRoom = "Living Room";

      currentTemperature = 72;
      targetTemperature = 72;

      surfaceMode = "Normal";

      floorLighting = false;
      automaticLighting = true;

      roomLights = true;
      curtainsOpen = true;

      workoutActive = false;
      workoutSpeed = 3.0;
      workoutDistance = 0;

      lastMovement = "2 minutes ago";
    }

    if (scenario === "Night Mode") {
      occupied = true;
      movementDetected = true;
      monitoredRoom = "Hallway";

      currentTemperature = 69;
      targetTemperature = 68;

      surfaceMode = "Soft";

      floorLighting = true;
      automaticLighting = true;

      roomLights = false;
      curtainsOpen = false;

      workoutActive = false;
      workoutSpeed = 3.0;
      workoutDistance = 0;

      lastMovement = "Just now";
    }

    if (scenario === "Workout Session") {
      occupied = true;
      movementDetected = true;
      monitoredRoom = "Workout Room";

      currentTemperature = 70;
      targetTemperature = 68;

      surfaceMode = "Exercise";

      floorLighting = true;
      roomLights = true;
      curtainsOpen = false;

      workoutActive = true;
      workoutSpeed = 3.5;
      workoutDistance = 0.8;

      lastMovement = "Continuous movement";
    }

    if (scenario === "Child Room") {
      occupied = true;
      movementDetected = true;
      monitoredRoom = "Child Bedroom";

      currentTemperature = 71;
      targetTemperature = 71;

      surfaceMode = "Soft";

      floorLighting = true;
      roomLights = false;
      curtainsOpen = false;

      workoutActive = false;
      workoutSpeed = 3.0;
      workoutDistance = 0;

      lastMovement = "30 seconds ago";
    }
  }

  function delay(ms) {
    return new Promise((resolve) => setTimeout(resolve, ms));
  }

  async function runSimulation() {
    if (simulationRunning) return;

    simulationRunning = true;
    selectedScenario = "Automatic Demo";

    occupied = false;
    movementDetected = false;
    roomLights = false;
    curtainsOpen = false;
    floorLighting = false;
    lastMovement = "Waiting for activity...";

    await delay(1200);

    occupied = true;
    movementDetected = true;
    lastMovement = "Person detected";

    await delay(1200);

    floorLighting = true;
    lastMovement = "Floor lighting activated";

    await delay(1200);

    roomLights = true;
    lastMovement = "Room lights activated";

    await delay(1200);

    curtainsOpen = true;
    lastMovement = "Curtains opened";

    await delay(1200);

    targetTemperature = 70;
    lastMovement = "Environment automatically adjusted";

    simulationRunning = false;
  }
</script>

<main class="app-container">
  <header>
    <h1>Smart Floor Interface</h1>
    <p>Interactive Smart Home Floor Prototype</p>
  </header>

  <div class="dashboard">
    <section class="device-panel">
      <div class="panel-heading">
        <div>
          <h2>Smart Floor Device UI</h2>
          <p class="subtitle">
            Current scenario: <strong>{selectedScenario}</strong>
          </p>
        </div>

        <div class:online={occupied} class="system-indicator">
          {occupied ? "ACTIVE" : "STANDBY"}
        </div>
      </div>

      <div class="floor-display">
        <h3>Smart Floor Status</h3>

        <div class="room-status-bar">
          <div class:status-on={roomLights}>
            Room Lights: {roomLights ? "ON" : "OFF"}
          </div>

          <div class:status-on={curtainsOpen}>
            Curtains: {curtainsOpen ? "OPEN" : "CLOSED"}
          </div>
        </div>

        <div class="floor-grid">
          <div class:movement-tile={movementDetected}></div>
          <div class:lighting-tile={floorLighting}></div>
          <div class:movement-tile={occupied}></div>

          <div class:lighting-tile={floorLighting}></div>
          <div class:movement-tile={movementDetected}></div>
          <div class:lighting-tile={floorLighting}></div>

          <div class:movement-tile={occupied}></div>
          <div class:lighting-tile={floorLighting}></div>
          <div class:movement-tile={movementDetected}></div>
        </div>

        <div class="floor-legend">
          <span>
            <span class="legend-box movement"></span>
            Movement / Occupancy
          </span>

          <span>
            <span class="legend-box lighting"></span>
            Floor Lighting
          </span>

          <span>
            <span class="legend-box inactive"></span>
            Inactive
          </span>
        </div>

        <div class="status-grid">
          <div>
            <span>Occupancy</span>
            <strong>{occupied ? "Occupied" : "Unoccupied"}</strong>
          </div>

          <div>
            <span>Movement</span>
            <strong>
              {movementDetected ? "Detected" : "None"}
            </strong>
          </div>

          <div>
            <span>Surface</span>
            <strong>{surfaceMode}</strong>
          </div>
        </div>
      </div>

      <div class="control-section">
        <h3>Temperature Control</h3>

        <div class="temperature-layout">
          <div>
            <span class="label">Current</span>
            <div class="big-value">
              {currentTemperature}°F
            </div>
          </div>

          <div>
            <span class="label">Target</span>

            <div class="temperature-buttons">
              <button on:click={decreaseTemperature}>
                −
              </button>

              <span>
                {targetTemperature}°F
              </span>

              <button on:click={increaseTemperature}>
                +
              </button>
            </div>
          </div>
        </div>

        {#if targetTemperature > currentTemperature}
          <p class="status-text">Heating...</p>
        {:else if targetTemperature < currentTemperature}
          <p class="status-text">Cooling...</p>
        {:else}
          <p class="status-text">Temperature Stable</p>
        {/if}
      </div>

      <div class="control-section">
        <h3>Surface Mode</h3>

        <p class="section-description">
          Adjust the floor surface for different activities.
        </p>

        <div class="button-row">
          <button
            class:active-button={surfaceMode === "Normal"}
            on:click={() => changeSurface("Normal")}
          >
            Normal
          </button>

          <button
            class:active-button={surfaceMode === "Soft"}
            on:click={() => changeSurface("Soft")}
          >
            Soft
          </button>

          <button
            class:active-button={surfaceMode === "Firm"}
            on:click={() => changeSurface("Firm")}
          >
            Firm
          </button>

          <button
            class:active-button={surfaceMode === "Exercise"}
            on:click={() => changeSurface("Exercise")}
          >
            Exercise
          </button>
        </div>

        <p>
          Active surface:
          <strong>{surfaceMode}</strong>
        </p>
      </div>

      <div class="control-section">
        <h3>Smart Home Controls</h3>

        <div class="setting-row">
          <div>
            <strong>Room Lights</strong>
            <p>{roomLights ? "Lights are on" : "Lights are off"}</p>
          </div>

          <button
            class:active-button={roomLights}
            on:click={() => (roomLights = !roomLights)}
          >
            {roomLights ? "ON" : "OFF"}
          </button>
        </div>

        <div class="setting-row">
          <div>
            <strong>Curtains</strong>
            <p>
              {curtainsOpen ? "Curtains are open" : "Curtains are closed"}
            </p>
          </div>

          <button
            class:active-button={curtainsOpen}
            on:click={() => (curtainsOpen = !curtainsOpen)}
          >
            {curtainsOpen ? "OPEN" : "CLOSED"}
          </button>
        </div>

        <div class="setting-row">
          <div>
            <strong>Movement Automation</strong>
            <p>
              Automatically control the room when movement is detected.
            </p>
          </div>

          <button
            class:active-button={automationEnabled}
            on:click={() => (automationEnabled = !automationEnabled)}
          >
            {automationEnabled ? "ENABLED" : "DISABLED"}
          </button>
        </div>

        <div class="setting-row">
          <div>
            <strong>Floor Lighting</strong>
            <p>
              Illuminate floor sections for visibility.
            </p>
          </div>

          <button
            class:active-button={floorLighting}
            on:click={() => (floorLighting = !floorLighting)}
          >
            {floorLighting ? "ON" : "OFF"}
          </button>
        </div>

        <div class="setting-row">
          <div>
            <strong>Automatic Floor Lighting</strong>
            <p>
              Activate floor lights when movement is detected.
            </p>
          </div>

          <button
            class:active-button={automaticLighting}
            on:click={() => (automaticLighting = !automaticLighting)}
          >
            {automaticLighting ? "ENABLED" : "DISABLED"}
          </button>
        </div>
      </div>

      <div class="control-section">
        <h3>Movement Monitoring</h3>

        <div class="monitor-box">
          <p>
            <span class="label">Monitoring:</span>
            <strong>{monitoredRoom}</strong>
          </p>

          <p>
            <span class="label">Movement:</span>
            <strong>
              {movementDetected ? "Detected" : "No Movement"}
            </strong>
          </p>

          <p>
            <span class="label">Last Activity:</span>
            <strong>{lastMovement}</strong>
          </p>
        </div>
      </div>

      <div class="control-section">
        <h3>Omnidirectional Workout Mode</h3>

        <p class="section-description">
          Simulates walking or running in any direction while remaining in one area.
        </p>

        <div class="workout-stats">
          <div>
            <span>Status</span>
            <strong>
              {workoutActive ? "ACTIVE" : "STOPPED"}
            </strong>
          </div>

          <div>
            <span>Speed</span>
            <strong>{workoutSpeed.toFixed(1)} MPH</strong>
          </div>

          <div>
            <span>Distance</span>
            <strong>{workoutDistance.toFixed(1)} mi</strong>
          </div>
        </div>

        <div class="speed-controls">
          <button on:click={decreaseSpeed}>
            − Speed
          </button>

          <button on:click={increaseSpeed}>
            + Speed
          </button>
        </div>

        <div class="button-row">
          <button class="start-button" on:click={startWorkout}>
            Start Workout
          </button>

          <button on:click={stopWorkout}>
            Stop
          </button>

          <button class="emergency-button" on:click={emergencyStop}>
            EMERGENCY STOP
          </button>
        </div>
      </div>
    </section>

    <section class="testing-panel">
      <h2>Testing / Simulation</h2>

      <p>
        <strong>Project:</strong>
        Smart Floor
      </p>

      <p>
        <strong>Created by:</strong>
        Santonio Gonzales
      </p>

      <a class="documentation-link" href="/">
        Project Documentation
      </a>

      <hr />

      <h3>Interface Location</h3>

      <p class="small-text">
        The main interface would be located on a wall-mounted
        control panel connected to the Smart Floor.
      </p>

      <div class="room-diagram">
        <div class="wall-panel">
          UI
        </div>

        <div class="mini-floor">
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
        </div>
      </div>

      <hr />

      <h3>Simulation Controls</h3>

      <button on:click={enterRoom}>
        Enter Room
      </button>

      <button on:click={leaveRoom}>
        Leave Room
      </button>

      <button on:click={walkAcrossFloor}>
        Walk Across Floor
      </button>

      <hr />

      <h3>Test Scenarios</h3>

      <button
        class:scenario-selected={selectedScenario === "Normal Day"}
        on:click={() => loadScenario("Normal Day")}
      >
        Normal Day
      </button>

      <button
        class:scenario-selected={selectedScenario === "Night Mode"}
        on:click={() => loadScenario("Night Mode")}
      >
        Night Mode
      </button>

      <button
        class:scenario-selected={selectedScenario === "Workout Session"}
        on:click={() => loadScenario("Workout Session")}
      >
        Workout Session
      </button>

      <button
        class:scenario-selected={selectedScenario === "Child Room"}
        on:click={() => loadScenario("Child Room")}
      >
        Child Room
      </button>

      <hr />

      <h3>Automatic Demonstration</h3>

      <p class="small-text">
        Simulate a person entering a Smart Floor room and watch
        the environment respond automatically.
      </p>

      <button
        class="demo-button"
        disabled={simulationRunning}
        on:click={runSimulation}
      >
        {simulationRunning
          ? "Simulation Running..."
          : "Run Smart Floor Demo"}
      </button>

      <hr />

      <button
        class="info-button"
        on:click={() => (showInfo = true)}
      >
        ? How to Use This Demo
      </button>
    </section>
  </div>
</main>

{#if showInfo}
  <div class="modal-background">
    <div class="modal">
      <h2>How to Use the Smart Floor Demo</h2>

      <p>
        This prototype simulates how a Smart Floor could respond
        to people and activities inside a room.
      </p>

      <ul>
        <li>
          Use <strong>Enter Room</strong> and
          <strong>Leave Room</strong> to simulate occupancy.
        </li>

        <li>
          Use <strong>Walk Across Floor</strong> to trigger
          movement sensors and automatic lighting.
        </li>

        <li>
          Select one of the four scenarios to see how the
          interface responds in different environments.
        </li>

        <li>
          Adjust the temperature, surface mode, lighting,
          automation, and workout controls directly in the
          Device UI.
        </li>

        <li>
          Run the automatic demonstration to watch several
          smart features respond in sequence.
        </li>
      </ul>

      <button
        class="close-button"
        on:click={() => (showInfo = false)}
      >
        Close
      </button>
    </div>
  </div>
{/if}