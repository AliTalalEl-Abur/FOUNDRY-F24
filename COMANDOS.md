# Comandos ejecutados - Foundry ZKsync

Este documento recoge los comandos principales ejecutados en este proyecto junto con su función.

## 1. Subir el proyecto a GitHub

Cambiar el remote `origin` al repositorio personal y subir la rama `main`:

```bash
git remote set-url origin https://github.com/AliTalalEl-Abur/FOUNDRY-F24.git
git push -u origin main
```

## 2. Instalar foundry-zksync

Descargar e instalar el toolkit (forge, cast, anvil-zksync) dentro de WSL/Ubuntu:

```bash
curl -L https://raw.githubusercontent.com/matter-labs/foundry-zksync/main/install-foundry-zksync | bash
```

> Nota: los binarios precompilados de foundry-zksync no están disponibles para Windows nativo. Se instalaron dentro de **WSL (Linux)**.

## 3. Compilar un contrato con ZKsync

```bash
forge build --zksync
```

Compila los contratos de `src/` usando `zksolc` y genera los artefactos en `zkout/`.

## 4. Iniciar un nodo local de ZKsync

```bash
anvil-zksync run
```

Levanta un nodo local en `http://localhost:8011` con:

- **Chain ID:** 260
- **Cuentas de prueba:** 10 cuentas con 10,000 ETH cada una
- **Mnemonic:** `test test test test test test test test test test test junk`

## 5. Desplegar un contrato al nodo local

```bash
forge create src/SimpleStorage.sol:SimpleStorage \
  --rpc-url http://localhost:8011 \
  --private-key 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80 \
  --zksync \
  --broadcast
```

### Datos del despliegue

| Campo | Valor |
|-------|-------|
| Contrato | `SimpleStorage` |
| Red | anvil-zksync (local) |
| RPC URL | `http://localhost:8011` |
| Chain ID | `260` |
| Deployer | `0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266` |
| Contrato desplegado | `0x588758d8a0Ad1162A6294f3C274753137E664aE0` |
| Transaction hash | `0x635dd5a88a51a43f3c50ceebeaf7a4b34c20995a8e9134fcba961abb1914310a` |
| Bloque | `1` |
| Gas usado | `1072239` |
| Effective gas price | `45250000` |
| Estado | `1` (éxito) |

> La clave privada usada es la primera **rich account** de anvil-zksync. Solo debe usarse en redes locales de prueba.

## 6. Consultar el receipt de una transacción

```bash
cast receipt 0x635dd5a88a51a43f3c50ceebeaf7a4b34c20995a8e9134fcba961abb1914310a \
  --rpc-url http://localhost:8011
```

Devuelve todos los detalles del despliegue: `blockHash`, `blockNumber`, `gasUsed`, `effectiveGasPrice`, `logsBloom`, `contractAddress`, etc.

## 7. Consultar los datos de una transacción

```bash
cast tx 0x635dd5a88a51a43f3c50ceebeaf7a4b34c20995a8e9134fcba961abb1914310a \
  --rpc-url http://localhost:8011
```

Muestra los campos de la transacción: `chainId`, `nonce`, `gasPrice`, `input`, `maxFeePerGas`, `value`, etc.

---

## Notas adicionales

- Los artefactos de compilación (`cache/`, `zkout/`) y los logs (`anvil-zksync.log`) están ignorados en Git mediante `.gitignore`.
- Para usar los binarios dentro de WSL: `forge`, `cast`, `anvil-zksync` y `foundryup-zksync` están disponibles globalmente.
