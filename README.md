example usage:

```
jobs:
 build-my-app:
   steps:
     <...>
     - name: Upload results
       uses: actions/upload-artifact@v3
       with:
        name: build
        path: dist
        retention-days: 2

  deploy-application-to-host:
    needs: build-my-app
    uses: "https://github.com/CuteHorse/DeployApplicationToHostAction/.github/workflows/DeployApplicationToHost.yml@master"
    with:
      host: '192.168.1.5'
      port: '22'
      user: 'root'
      app-dir: ${{vars.APP_DIR}}
      tmp-dir: '/tmp/deploy'
      pre-copy: 'systemctl stop MyAppServiceName'
      post-copy: 'chown -R www-data ${{vars.APP_DIR}} && chmod +x ${{vars.APP_DIR}}/MyAppExecutable && systemctl start MyAppServiceName'
    secrets:
      private-key: ${{ secrets.SSH_PRIVATE_KEY }}
```
