# Sombrero Studio

A Minecraft development monorepo.

## Structure

```
├── plugins/     # Spigot/Paper plugins
├── mods/        # Fabric/Forge mods
├── servers/     # Server configs and deployments
└── shared/      # Shared libraries and utilities
```

## Getting Started

### Prerequisites
- JDK 21+
- Gradle 9.x / Maven 3.x

### Building
```bash
# Build everything
./gradlew build

# Build a specific module
./gradlew :plugins:<module>:build
```

## License

Proprietary — Sombrero Studio
