# H1 Github-actions-markdown

'code'
- name: Dotnet test
        run: dotnet test --configuration ${{ env.dotnetBuildConfiguration }} --collect "Code coverage"
        shell: pwsh
