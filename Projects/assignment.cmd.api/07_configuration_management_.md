# Chapter 7: Configuration Management

Welcome back! In the previous chapter, [Kafka Client (MicroservicesClient)](06_kafka_client__microservicesclient__.md), we saw how our application sends messages to other services using Kafka. We noted that the connection details for Kafka (like server addresses) came from a `config` object. But how does the application know *which* Kafka servers to connect to? The ones we use for local development are different from the ones used in the live production environment!

This chapter explores **Configuration Management**, the system our application uses to handle settings that change depending on where and how it's running.

## What's the Goal? Adapting Settings for Different Situations

Imagine you're building a website. When you're working on it locally on your laptop (the **development** environment), you might connect to a test database running on your machine. When your teammates are testing it before release (the **staging** environment), it needs to connect to a shared test database. Finally, when the website goes live for real users (the **production** environment), it must connect to the secure, powerful production database.

You don't want to change the application's code every time you deploy it to a different environment! You need a way to tell the application, "Okay, you're running in *production* now, so use *this* database address and *these* Kafka server details."

**Configuration Management** is how our `assignment.cmd.api` handles this. It allows us to define different settings for different environments without touching the core application logic. Think of it as the application's adjustable settings panel.

## Key Concepts: The Settings Panel

1.  **Configuration:** These are the settings our application needs that can vary. Examples include:
    *   Database connection strings (like the address for our [Event Sourcing & Repositories](05_event_sourcing___repositories_.md) MongoDB store).
    *   Kafka broker addresses (for [Command Handling (Controllers & Commands)](01_command_handling__controllers___commands__.md) and the [Kafka Client (MicroservicesClient)](06_kafka_client__microservicesclient_.md)).
    *   Network ports (like the one the web server part listens on).
    *   Secret keys (like API keys or encryption keys - often handled securely).

2.  **Environments:** We typically have different environments where the application runs:
    *   `development`: Your local machine, for coding and initial testing.
    *   `staging`: A test environment that closely mimics production, used for final testing before release.
    *   `production`: The live environment where real users interact with the application.

3.  **Sources of Configuration:** Where do these settings come from? Our application uses a library called `nconf` to gather settings from multiple places in a specific order:
    *   **Command-line Arguments:** Settings passed directly when starting the application (e.g., `node main.js --PORT=8080`).
    *   **Environment Variables:** Settings configured in the operating system where the application runs (e.g., `export KAFKA_URL=...`). These are very common in deployment environments like Docker or AWS ECS.
    *   **Environment-Specific Files:** We have files like `development.ts`, `staging.ts`, and `production.ts` inside `src/config/env/`. These contain the default settings for each environment.
    *   **Static File:** A base `static/config.ts` file might hold settings that rarely change across environments.

4.  **Hierarchy (Order of Importance):** `nconf` merges these sources, with later sources overriding earlier ones. The typical priority order is:
    1.  Command-line arguments (Highest priority - overrides everything else).
    2.  Environment variables.
    3.  The specific environment file (e.g., `production.ts`).
    4.  The static file (Lowest priority - base defaults).

## How It Works: Loading the Right Settings

When the `assignment.cmd.api` application starts up, here's how it figures out its configuration:

1.  **Check Environment:** It first looks for a setting called `NODE_ENV`. This is usually provided as an environment variable (like `NODE_ENV=production`). If not found, it defaults to `development`.
2.  **Load Base Settings:** It might load some default settings from a static file.
3.  **Load Environment File:** Based on the `NODE_ENV` value, it loads the corresponding file (e.g., if `NODE_ENV` is `production`, it loads `src/config/env/production.ts`). These settings override the base settings.
4.  **Load Env Vars & Args:** It then loads any settings provided as environment variables or command-line arguments. These override any settings loaded from the files.
5.  **Final Config Object:** The result is a single `config` object containing the merged settings, prioritized correctly. This object is then used throughout the application.

Here's a simple diagram showing the layering:

```mermaid
graph TD
    subgraph "Sources (Lower Priority First)"
        direction TB
        Static[Static Config File<br>(e.g., static/config.ts)]
        EnvFile[Environment File<br>(e.g., production.ts)]
        EnvVars[Environment Variables<br>(e.g., KAFKA_NODE_1)]
        CmdArgs[Command-Line Arguments<br>(e.g., --PORT=8080)]
    end

    subgraph "Loading Process (Using nconf)"
        direction TB
        LoadStatic(Load Static File)
        LoadEnvFile(Load Env File based on NODE_ENV)
        LoadEnvVars(Load Environment Variables)
        LoadCmdArgs(Load Command-Line Args)
    end

    subgraph "Result"
        FinalConfig[Final Config Object]
    end

    Static --> LoadStatic
    LoadStatic --> LoadEnvFile
    EnvFile --> LoadEnvFile
    LoadEnvFile --> LoadEnvVars
    EnvVars --> LoadEnvVars
    LoadEnvVars --> LoadCmdArgs
    CmdArgs --> LoadCmdArgs
    LoadCmdArgs --> FinalConfig

    style Static fill:#f9f,stroke:#333,stroke-width:2px
    style EnvFile fill:#ccf,stroke:#333,stroke-width:2px
    style EnvVars fill:#fcf,stroke:#333,stroke-width:2px
    style CmdArgs fill:#ffc,stroke:#333,stroke-width:2px
```

## Looking at the Code

Let's see how this is implemented.

**1. The Main Config Loader (`src/config/index.ts`)**

This file orchestrates the loading process using `nconf`.

```typescript
import * as nconf from 'nconf';
import * as _ from 'lodash';
import { staticConfig } from './static/config'; // Base settings

// 1. Tell nconf to read from env vars and cmd args
nconf.env().argv();

// 2. Determine the environment (default to 'development')
const environment = nconf.get('NODE_ENV') || 'development';

// 3. Load and merge settings
export default _.extend( // Uses lodash utility to merge objects
  { environment }, // Add the determined environment name
  staticConfig, // Start with static defaults
  // Load the environment-specific file
  require(`${__dirname}/env/${environment}`),
  nconf.get(), // Finally, layer env vars/args on top
);
```

*   **Explanation:**
    *   `nconf.env().argv();`: Tells `nconf` to be aware of environment variables and command-line arguments first.
    *   `nconf.get('NODE_ENV')`: Reads the `NODE_ENV` value (which might come from an env var).
    *   `require(...)`: Dynamically loads the correct `.ts` file (e.g., `development.ts`, `production.ts`) based on the `environment`.
    *   `_.extend(...)`: Merges all the loaded settings together. Because `nconf.get()` (containing env vars/args) is last, it gets the highest priority.

**2. Example Environment File (`src/config/env/development.ts`)**

This file defines settings specifically for local development.

```typescript
/** Development specific config */
import {Config} from '../interfaces';
import {buildConfiguration} from "@haulapp/common-types-and-helpers";

// Local Kafka address
const kafkaBrokers: string[] =  ['localhost:9092'];

const config: Config = {
    http: { // Web server port
        port: process.env.PORT || 3007,
    },
    // Local MongoDB address for event store
    event_store_url: 'mongodb://localhost:27017/assignments-eventstore',
    kafkaSettings: { // Kafka settings for development
        server: buildConfiguration(kafkaBrokers, false).assignmentCmdApi,
        client: buildConfiguration(kafkaBrokers, true).assignmentCmdApi,
    },
};
module.exports = config; // Export the config object
```

*   **Explanation:** This file sets the `event_store_url` to a local MongoDB instance and `kafkaBrokers` to `localhost:9092`. The `buildConfiguration` helper likely formats these addresses correctly for the Kafka client library.

**3. Another Environment File (`src/config/env/production.ts`)**

This file defines settings for the live production environment. Notice how it uses `process.env` to get values likely injected during deployment.

```typescript
/** Production specific config */
import {Config} from '../interfaces';
import {buildConfiguration} from "@haulapp/common-types-and-helpers";

// Get Kafka addresses from environment variables
const kafkaBrokers = [
  process.env.PRODUCTION_KAFKA_NODE_1,
  process.env.PRODUCTION_KAFKA_NODE_2,
  process.env.PRODUCTION_KAFKA_NODE_3
];

const config: Config = {
  http: { port: process.env.PORT || 3007 },
  // Get Event Store URL from an environment variable
  event_store_url: process.env.ASSIGNMENT_EVENT_STORE_URL_PRODUCTION,
  kafkaSettings: { // Use production Kafka broker settings
    server: buildConfiguration(kafkaBrokers, false).assignmentCmdApi,
    client: buildConfiguration(kafkaBrokers, true).assignmentCmdApi,
  },
};
module.exports = config;
```

*   **Explanation:** This file relies heavily on `process.env.VAR_NAME` to get crucial connection details like the production Event Store URL (`ASSIGNMENT_EVENT_STORE_URL_PRODUCTION`) and the Kafka broker addresses (`PRODUCTION_KAFKA_NODE_1`, etc.). These environment variables are set *outside* the code, typically during the deployment process.

**4. Using the Configuration (`src/main.ts`)**

The rest of the application imports the final `config` object from `src/config/index.ts` and uses its values.

```typescript
// src/main.ts
import { NestFactory } from '@nestjs/core';
import { MicroserviceOptions, Transport } from '@nestjs/microservices';
import { AppModule } from './app.module';
// Import the final config object
import config from 'src/config';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // Use config for Kafka connection
  app.connectMicroservice<MicroserviceOptions>({
    transport: Transport.KAFKA,
    options: config.kafkaSettings.server, // <-- Config value used!
  });

  // ... other setup ...

  await app.startAllMicroservicesAsync();
  // Use config for HTTP port
  await app.listen(config.http.port); // <-- Config value used!
  console.log(`App listening on port: ${config.http.port}`);
}
bootstrap();
```

*   **Explanation:** The `main.ts` file imports the `config` object. It then uses `config.kafkaSettings.server` to connect to the correct Kafka brokers and `config.http.port` to listen on the correct network port, all based on the loaded configuration for the current environment.

## Configuration in Deployment

Where do the environment variables like `PRODUCTION_KAFKA_NODE_1` or `ASSIGNMENT_EVENT_STORE_URL_PRODUCTION` actually come from when running in staging or production?

They are typically injected by the deployment system. In our project, we use AWS Elastic Container Service (ECS). Looking at the task definition files (`task-def-staging.json` and `task-def-production.json`) provided earlier, you can see sections like `environment` and `secrets`:

*   `environment`: Sets basic environment variables like `NODE_ENV`.
    ```json
      "environment": [
        {
          "name": "NODE_ENV",
          "value": "production" // Or "staging"
        }
      ]
    ```
*   `secrets`: Pulls sensitive values (like database URLs, Kafka addresses, API keys) securely from a secrets manager (like AWS Systems Manager Parameter Store) and injects them as environment variables into the running container.
    ```json
      "secrets": [
        // ... other secrets ...
        {
          "valueFrom": "arn:aws:ssm:us-west-2:333759594765:parameter/ASSIGNMENT_EVENT_STORE_URL_PRODUCTION",
          "name": "ASSIGNMENT_EVENT_STORE_URL_PRODUCTION"
        },
        {
          "valueFrom": "arn:aws:ssm:us-west-2:333759594765:parameter/PRODUCTION_KAFKA_NODE_1",
          "name": "PRODUCTION_KAFKA_NODE_1"
        }
        // ... more secrets ...
      ]
    ```

This way, the sensitive production settings are never stored directly in the code repository. The application code (`production.ts`) just expects them to be present as environment variables when it runs in the production environment.

## Conclusion

You've now learned about **Configuration Management** in `assignment.cmd.api`!

*   It allows the application to adapt its settings (like database URLs, Kafka brokers, ports) based on the environment (`development`, `staging`, `production`).
*   It uses the `nconf` library to load settings from **files**, **environment variables**, and **command-line arguments**.
*   There's a clear **hierarchy**, with environment variables and command-line arguments overriding file settings.
*   Environment-specific files (`development.ts`, `production.ts`, etc.) define the defaults for each environment.
*   In deployment (like AWS ECS), crucial settings and secrets are often injected as **environment variables**.
*   The final merged `config` object is used throughout the application to access the correct settings.

This keeps our application flexible and avoids hardcoding settings that need to change between environments. We saw how environment variables and secrets are managed in deployment definition files.

In the final chapter, we'll take a closer look at one of those deployment files: the [ECS Task Definition](08_ecs_task_definition_.md), to understand how our application is actually packaged and run in the cloud.

---

Generated by [AI Codebase Knowledge Builder](https://github.com/The-Pocket/Tutorial-Codebase-Knowledge)