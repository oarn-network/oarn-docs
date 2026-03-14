# Contributing to OARN Network

Thank you for your interest in contributing to the OARN Network! This document provides guidelines for contributing to our open-source project.

## Code of Conduct

Be respectful and constructive. We welcome contributors from all backgrounds and experience levels.

## Ways to Contribute

### 1. Code Contributions

#### Node Software (Rust)
- Repository: [oarn-node](https://github.com/oarn-network/oarn-node)
- Language: Rust
- Key areas: P2P networking, ONNX inference, blockchain integration

```bash
git clone https://github.com/oarn-network/oarn-node
cd oarn-node
cargo build
cargo test
```

#### Smart Contracts (Solidity)
- Repository: [oarn-contracts](https://github.com/oarn-network/oarn-contracts)
- Language: Solidity 0.8.24
- Key areas: TaskRegistry, Governance, Token contracts

```bash
git clone https://github.com/oarn-network/oarn-contracts
cd oarn-contracts
npm install
npx hardhat test
```

#### SDK (TypeScript)
- Repository: [oarn-sdk](https://github.com/oarn-network/oarn-sdk)
- Language: TypeScript
- Key areas: Client library, batch processing, IPFS integration

```bash
git clone https://github.com/oarn-network/oarn-sdk
cd oarn-sdk
npm install
npm run build
npm test
```

### 2. Documentation

- Repository: [oarn-docs](https://github.com/oarn-network/oarn-docs)
- Fix typos, improve explanations, add examples
- Translate documentation to other languages

### 3. Testing

- Run nodes on testnet and report issues
- Test edge cases and document findings
- Write unit tests for existing code

### 4. Bug Reports

Open an issue with:
- Clear description of the bug
- Steps to reproduce
- Expected vs actual behavior
- Environment details (OS, versions)

### 5. Feature Requests

Open an issue with:
- Use case description
- Proposed solution
- Alternatives considered

## Development Workflow

### 1. Fork and Clone

```bash
# Fork the repository on GitHub, then:
git clone https://github.com/YOUR_USERNAME/oarn-node
cd oarn-node
git remote add upstream https://github.com/oarn-network/oarn-node
```

### 2. Create a Branch

```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/issue-description
```

### 3. Make Changes

- Follow existing code style
- Add tests for new functionality
- Update documentation as needed

### 4. Test Your Changes

```bash
# Rust
cargo test
cargo clippy

# Solidity
npx hardhat test

# TypeScript
npm test
```

### 5. Commit

Use clear, descriptive commit messages:

```
feat: add batch task submission to SDK
fix: resolve consensus race condition in V2
docs: update node operator guide
test: add integration tests for IPFS
```

### 6. Push and Create PR

```bash
git push origin feature/your-feature-name
```

Then open a Pull Request on GitHub.

## Pull Request Guidelines

- **Title**: Clear, concise description
- **Description**: Explain what and why
- **Tests**: Include relevant tests
- **Documentation**: Update if needed
- **Single Focus**: One feature/fix per PR

## Code Style

### Rust
- Use `rustfmt` for formatting
- Run `cargo clippy` for lints
- Follow Rust API Guidelines

### Solidity
- Use Prettier with Solidity plugin
- Follow Solidity Style Guide
- Prefer explicit over implicit

### TypeScript
- Use ESLint and Prettier
- Prefer typed functions
- Document public APIs

## Getting Help

- **Discord**: https://discord.gg/RsrQwNvt
- **GitHub Discussions**: Ask questions, share ideas
- **Twitter**: https://twitter.com/OARNNetwork

## Recognition

Contributors are recognized in:
- Release notes
- Contributors file
- Future GOV token distribution (for significant contributions)

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

Thank you for contributing to decentralized AI infrastructure!
