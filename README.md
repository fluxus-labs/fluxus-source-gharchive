# fluxus-source-gharchive

A Fluxus source component for processing and analyzing GitHub Archive data streams, providing efficient access to historical GitHub event data.

## Overview

fluxus-source-gharchive is a powerful Rust library that enables seamless integration with GitHub Archive data. It provides a robust interface for streaming and processing historical GitHub events, supporting both HTTP-based remote access and local file processing.

## Features

- **Flexible Data Source Support**
  - HTTP streaming from gharchive.org
  - Local file processing for offline analysis
  - Automatic handling of gzip compression

- **Advanced Time Range Control**
  - Date-based data retrieval (YYYY-MM-DD format)
  - Hour-specific data access (0-23 hour range)
  - Configurable date ranges with start and end dates

- **Comprehensive Event Data**
  - Full GitHub event information including:
    - Event type and ID
    - Repository details
    - Actor information
    - Organization data
    - Event payload
    - Timestamps

- **Robust Error Handling**
  - Configurable I/O timeouts
  - Detailed error reporting
  - Stream-based error handling

## Installation

Add this to your `Cargo.toml`:

```toml
[dependencies]
fluxus-source-gharchive = "0.1.0"
```

## Usage

### Basic HTTP Source

```rust
use fluxus_source_gharchive::GithubArchiveSource;
use fluxus::sources::Source;

#[tokio::main]
async fn main() {
    // Create a source for a specific hour
    let uri = "https://data.gharchive.org/2015-01-01-15.json.gz";
    let mut source = GithubArchiveSource::new(uri).unwrap();
    
    // Configure timeout
    source.set_io_timeout(std::time::Duration::from_secs(20));
    
    // Initialize the source
    source.init().await.unwrap();
    
    // Process events
    while let Ok(Some(event)) = source.next().await {
        println!("Event: {:?}", event);
    }
}
```

### Date Range Processing

```rust
use fluxus_source_gharchive::GithubArchiveSource;
use fluxus::sources::Source;

#[tokio::main]
async fn main() {
    // Create a source starting from a specific date
    let mut source = GithubArchiveSource::from_date("2021-01-01").unwrap();
    
    // Set end date (optional)
    source.set_end_date("2021-01-02").unwrap();
    
    // Initialize and process
    source.init().await.unwrap();
    while let Ok(Some(event)) = source.next().await {
        println!("Event: {:?}", event);
    }
}
```

### Local File Processing

```rust
use fluxus_source_gharchive::GithubArchiveSource;
use fluxus::sources::Source;
use std::path::Path;

#[tokio::main]
async fn main() {
    // Create a source from a local file
    let path = Path::new("path/to/your/archive.json.gz");
    let mut source = GithubArchiveSource::from_file(path).unwrap();
    
    // Initialize and process
    source.init().await.unwrap();
    while let Ok(Some(event)) = source.next().await {
        println!("Event: {:?}", event);
    }
}
```

## License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.