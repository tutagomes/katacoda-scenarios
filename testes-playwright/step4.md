Para começar a criar nosso primeiro caso de teste, vamos abrir o arquivo criado anteriormente:

`tests/example.spec.ts`{{open}}

E então, adicionar o conteúdo:

```js
import { test, expect } from '@playwright/test';

test('teste inicial', async ({ page }) => {
  // Navegando até a página que queremos testar
  await page.goto('http://localhost');
  // Buscando o seletor de título
  const title = page.locator('.card .card-content .title');
  // Esperando que o título contenha "Tarefa"
  await expect(title).toHaveText('Tarefas');
  // Esperando que o título não contenha um "Titulo Ruim"
  await expect(title).not.toHaveText('Titulo Ruim');
});

test.fixme('teste de novo titulo', async ({ page }) => {
  // Navegando até a página que queremos testar
  await page.goto('http://localhost')
  //Buscando seletor de título
  const title = page.locator('.card .card-content .title');
  // Esperando que o título não contenha "Tarefas Legais" e o teste falhe
  await expect(title).toHaveText('Tarefas Legais');
})

```

[Referência de sintaxe de teste](https://playwright.dev/docs/test-annotations)



Agora, vamos criar um novo teste, aproveitando e adicionando interações com o navegador:

`touch tests/tarefas.spec.ts`{{execute}}

`tests/tarefas.spec.ts`{{open}}

```ts
import { test, expect } from '@playwright/test';

test.describe('testes criação de tarefas', () => {
  test('criar tarefa clicando no botao', async ({ page }) => {
    // Navegando até a página que queremos testar
    await page.goto('http://localhost');
    // Preenchendo campo
    await page.fill('.field .control input', 'Minha Tarefa');
    // Clicando no botão de adicionar
    const botao = page.locator('.field .control .button');
    botao.click();
    // Esperamos que o campo esteja vazio
    await page.screenshot({ path: 'screenshot.png', fullPage: true });
    const campo = page.locator('.field .control input');
    await expect(campo).toBeEmpty();
    await page.waitForSelector('.media > .media-content > .content > p > strong');
    // E a tarefa esteja criada
    const tarefa = page.locator('.media > .media-content > .content > p > strong');
    await expect(tarefa).toHaveText('Minha Tarefa');
  });

  test('criar tarefa apertando enter', async ({ page }) => {
    // Navegando até a página que queremos testar
    await page.goto('http://localhost');
    // Preenchendo campo
    await page.fill('.field .control input', 'Minha Tarefa');
    // Apertando enter
    await page.keyboard.press('Enter');
    // Esperamos que o campo esteja vazio
    const campo = page.locator('.field .control input');
    await expect(campo).toBeEmpty();
		// E a tarefa esteja la
    await page.waitForSelector('.media > .media-content > .content > p > strong');
    const tarefa = page.locator('.media > .media-content > .content > p > strong');
    await expect(tarefa).toHaveText('Minha Tarefa');
  });
});

```



Links úteis:

- [Sintaxe de seletores](https://playwright.dev/docs/selectors)
- [Sintaxe de Inputs](https://playwright.dev/docs/input)
- [Sintaxe de Arquivos](https://playwright.dev/docs/downloads)
- [Sintaxe de Capturas de Tela](https://playwright.dev/docs/screenshots)
- [Sintaxe de Vídeos](https://playwright.dev/docs/videos)



Agora, vamos criar um código para testar se, ao clicar em uma tarefa, a caixa de seleção do lado esquerdo se torna preenchida.

