Para resolver o problema do passo anterior, podemos utilizar o código abaixo:

`touch tests/tarefa-done.spec.ts`{{execute}}

`tests/tarefa-done`{{open}}



```ts
test('marcar como concluida', async ({ page }) => {
      await page.goto('http://localhost')

      await page.fill('.field .control input', 'Minha Tarefa');
      // Apertando enter
      await page.keyboard.press('Enter');
      // Esperamos que o campo esteja vazio
      const campo = page.locator('.field .control input');
      await expect(campo).toBeEmpty();
      // E a tarefa esteja la
      await page.waitForSelector('.media > .media-content > .content > p > strong');
      const tarefa = page.locator('.media > .media-content > .content > p > strong');
      tarefa.click()
      const checkbox = page.locator('.media > .media-left input')
      await expect(checkbox).toBeChecked()
      // Ao clicar novamente, não estar marcada
      tarefa.click()
      await expect(checkbox).not.toBeChecked()
  })
```

E, por último, vamos criar um código testar a exclusão de uma tarefa!





### Links úteis:

Com o framework, também é possível gravar os testes, utilizando o [Playwright Test Generator](https://playwright.dev/docs/codegen).

Naturalmente, nem sempre é interessante abrir um novo navegador toda a vez que um teste seja executado, principalmente quando é necessário manter uma sessão de usuário.

Para isso, o pessoal do Playwright criou uma maneira de salvar e carregar contextos de usuário: [Autenticação com Playwright](https://playwright.dev/docs/test-auth/) .

