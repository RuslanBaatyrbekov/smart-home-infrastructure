# Smart Home Infrastructure Configuration

This repository contains the containerized multi-service configuration for a local smart home server built with Home Assistant, ESPHome, local Voice Assistant pipelines (Wyoming openWakeWord/Piper/Whisper), and Music Assistant.

## Directory Structure
* `ha_config/` - Home Assistant configuration volume.
* `esphome_config/` - ESPHome configuration volume for microcontroller nodes.
* `mass_data/`, `piper_data/`, `whisper_data/`, `wyoming/` - Persistent data directories for media and AI models (excluded from Git tracking).

## Portability & Secrets Management
All sensitive variables (Wi-Fi credentials, static IP configurations, entity names, and personal LLM prompts) are strictly modularized into local `secrets.yaml` files inside `ha_config/` and `esphome_config/`. These files are ignored by Git.

### Migration Guide to a New Server or Router
If you switch to a new mini-server or deploy a new Wi-Fi router, follow these steps:

1. **Clone this repository** to the new host machine.
2. **Recreate the private configuration files:**
    * Create `ha_config/secrets.yaml`
    * Create `esphome_config/secrets.yaml`
3. **Populate network data:** Fill both `secrets.yaml` files with your new Wi-Fi credentials, internal device IPs, and custom text strings.
4. **Router Provisioning:** Ensure that the monitored smartphone has a static DHCP reservation matching the IP configured in `ha_config/secrets.yaml`.
5. **Launch Stack:** Run `docker compose up -d` in the root directory.
6. **Reflash ESPHome Nodes:** Open the ESPHome dashboard (`:6052`) and update the nodes. They will compile using the new credentials automatically.