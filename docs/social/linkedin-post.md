I've been building shared AI infrastructure for creative studios, with Houdini and After Effects as the first two application clients.

The main achievement for me is the infrastructure: one studio-managed endpoint that brings together artist access, model configuration and local or cloud inference.

The stack combines:

• Open WebUI for artist accounts, user API keys and model access.
• LiteLLM for model aliases, provider credentials and routing.
• PostgreSQL for persistent LiteLLM configuration.
• Ollama for local inference, alongside configured external cloud providers.
• Docker Compose / Portainer for deployment, with persistent service volumes.

Artists connect using their Open WebUI user key. Provider access is configured in the shared service. Model routes can be configured centrally while each application keeps its own tools, project history and execution permissions.

Hou-lm and Ae-lm put that infrastructure to work inside Houdini and After Effects. They send relevant context and permitted images, execute locally approved scripts, and return results for further model steps.

This is the foundation I want to build on as more creative applications are connected. The repositories are open source under MIT; Ae-lm is currently a development preview.

Infrastructure: https://github.com/Byter3/studio-assistant-backend
Houdini client: https://github.com/Byter3/Hou-lm
After Effects client: https://github.com/Byter3/Ae-lm

#StudioPipeline #AIInfrastructure #Houdini #AfterEffects #OpenSource
