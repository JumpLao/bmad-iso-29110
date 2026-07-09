# BMAD ISO 29110 Extension - GitHub Setup

## 1. Login to GitHub

```bash
gh auth login
# Follow the OAuth flow
```

## 2. Create Public Repository

```bash
cd /home/durian/projects/bmad-iso-29110
gh repo create nousresearch/bmad-iso-29110 --public --source . --push --description "BMAD-METHOD extension for ISO/IEC 29110 Basic Profile compliance. Adds process documentation, traceability matrix, risk register, and compliance audit to any BMAD project. Install via npx bmad-method install --modules iso-29110."
```

## 3. Verify Repository

```bash
gh repo view nousresearch/bmad-iso-29110 --web
```

This will open the repository in browser.

## Repository Structure

- 13 files committed (b48ae7b)
- 2 BMAD skills (closure, generator)
- 7 ISO 29110 templates (647 lines total)
- Module config + marketplace.json
- README with installation guide

After push, repository will be at: https://github.com/nousresearch/bmad-iso-29110

Ready to use in any BMAD project via:
```bash
npx bmad-method install --modules bmm,iso-29110 --yes
```