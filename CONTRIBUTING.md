# CONTRIBUTING

Contributions are always welcome, no matter how large or small. Before contributing, please read the [code of conduct](CODE_OF_CONDUCT.md).

## Setup

```bash
git clone <your-fork-url>
cd 1clik-kwriding-cms
git submodule update --init --recursive
yarn install
```

## Building

```bash
yarn build
```

Deploy-preview build (includes drafts + future content):

```bash
yarn build:preview
```

## Development

```bash
yarn start
```

## Pull Requests

We actively welcome pull requests.

1. Fork the repo and create your branch from `main`.
2. Keep changes focused and describe the intent in the PR description.
3. If you changed content structure or build behavior, update `README.md` and/or `agents.md`.
4. Ensure `yarn build:preview` passes.

## License

By contributing to KW Riding, you agree that your contributions will be licensed under its [MIT license](LICENSE).
