# Relatório 4 - *Verification and Validation*

O objetivo deste relatório é documentar o estado atual do projeto no que respeita à verificação e validação. Numa primeira parte, será feita uma análise relativamente ao grau de testabilidade do projeto, relatando a forma de testar os componentes da aplicação, bem como esta podia ser melhorada. De seguida, são apresentadas algumas estatísticas de teste, relativamente ao número de testes e à cobertura. Por fim, é explicada a forma como resolvemos o *bug* escolhido.

## *Software Testability and Reviews*

O uso de testes é uma mais valia para garantir a qualidade do projeto, pois permitem determinar a existência de erros importantes. No entanto, os testes não provam que o código não tenha falhas.

### Controlabilidade
A controlabilidade, por definição, é o grau que permite controlar o estado do componente a ser testado (CUT - *Component Under Test*).

Analisando os testes do *OwnCloud*, verificamos que são bastantes específicos, ou seja, a sua controlabilidade é tanto maior quanto a especifidade do teste.

### Observabilidade
Este ponto refere-se ao grau no qual é possível observar os resultados intermediários e finais dos testes.

Para obter o resultado dos testes ao nível da camada da aplicação, é necessário corrê-los no *Android Studio*. Os testes que exercitam as operações no servidor, devem ser corridos através do terminal, sendo para isso necessário possuir o *JUnit* e o *Apache Ant*. 

No entanto, alguns testes que estão no *branch master* (nomeadamente os que se encontram na pasta *automationTest*) estão obsoletos. Segundo a informação obtida junto do atual responsável pela aplicação, a equipa tem como objetivo produzir um conjunto mais alargado de testes *Espresso* que permitam abranger quase toda a aplicação. Existe já um *branch* denominado *login_suite_espresso* onde estes testes se encontram a ser produzidos, mas não estão prontos para ser testados e, por esta razão, o grupo decidiu não os incluir neste relatório.

### Isolabilidade
A isolabilidade representa o grau em que cada componente pode ser testado isoladamente.

Assim, a isolabilidade é tanto maior quanto menos se relacionem os módulos uns com os outros. No caso do *OwnCloud*, a maior parte dos módulos estão relacionados entre si, o que dificulta o teste de cada módulo isoladamente.

### Separação de Responsabilidades
A separação de responsabilidades define se o componente a ser testado tem uma responsabilidade bem definida.

Para que a estrutura do projeto fique bem organizada e de fácil compreensão e acesso, cada módulo deve estar bem definido, evitando assim que o código fique misturado e menos eficiente. No caso do *OwnCloud*, a sua estrutura está bem definida. Os principais desenvolvedores optaram por criar vários *packages* de forma a que cada funcionalidade fique bem definida, sendo os seus sub-problemas resolvidos no seu interior.

### Perceptibilidade
A perceptibilidade avalia o grau em que o componente em teste está autoexplicativo e documentado.

Avaliando os testes disponíveis no projeto, determinamos que o nome dos mesmo é claro e, por isso, autoexplicativo. Isto permite ao utilizador verificar com muita facilidade qual o teste que falhou e a localização do erro.

### Heterogeneidade
Determina o grau que o uso de diversas tecnologias requer, para usar diversos métodos de ensaio e ferramentas em paralelo.

No momento da avaliação do projecto, tinham sido usados testes em *Apache Ant/JUnit3* na versão inicial do projecto, *Gradle* quando mudaram para o *Android Studio* como principal IDE, *Appium* numa primeira fase de ínicio dos testes da *UI*, e Espresso para testar o *login*. Cada um foi usado num diferente período do projecto, estando os métodos desatualizados sendo apenas o Espresso atualizado e o método que irá permanecer como ferramenta de testes. Assim concluimos que o projecto *OwnCloud* atualmente não é heterogéneo pois embora tenham sido usadas várias ferramentas atualmente apenas uma é usada.

## *Report Test Statistics and analytics*

De uma forma geral, para avaliar a qualidade do *software*, recorrem-se a estatísticas de teste que tentam contemplar o maior número de componentes possível, determinando a eficiência e estabilidade do sistema. 

No caso particular do *ownCloud*, como foi referido no tópico anterior, o número de casos de teste é muito reduzido, havendo, por isso, uma percentagem de cobertura também muito reduzida.

Ao nível da camada de aplicação, existem apenas 5 testes que incidem sobre os pacotes *authentication*, *datamodel*, *uiautomator*, produzindo o seguinte resultado:

![GradleTests](/ESOF-docs/resources/gradle_tests.png)

Além dos testes ao nível da camada da aplicação, existe um outro conjunto de testes baseados em *JUnit3*, que exercitam a maior parte das operações que são possíveis realizar num servidor real do *ownCloud*. Para correr estes testes, foi necessário a instalação do *Apache Ant*, bem como a definição de algumas variáveis de ambiente requiridas pelos mesmos. 

Uma vez que a equipa segue uma prática de integração contínua, recorre ao [**Travis CI**](https://travis-ci.org/owncloud/android) para que sempre que é realizado um *pull request*, estes testes serem corridos, garantindo que as alterações que o código sofreu não alteraram estas funcionalidades.

O grupo correu estes testes, e obteve o seguinte resultado:

```
-setup:
     [echo] Project Name: ownCloud Android library test cases
  [gettype] Project Type: Test Application

-test-project-check:

test:
     [echo] Running tests ...
     [exec] 
     [exec] com.owncloud.android.lib.test_project.test.CopyFileTest:.
     [exec] com.owncloud.android.lib.test_project.test.CreateFolderTest:..
     [exec] com.owncloud.android.lib.test_project.test.CreateShareTest:
     [exec] Failure in testCreateFederatedShareWithUser:
     [exec] junit.framework.AssertionFailedError
     [exec] 	at com.owncloud.android.lib.test_project.test.CreateShareTest.testCreateFederatedShareWithUser(CreateShareTest.java:238)
     [exec] 	at android.test.InstrumentationTestCase.runMethod(InstrumentationTestCase.java:214)
     [exec] 	at android.test.InstrumentationTestCase.runTest(InstrumentationTestCase.java:199)
     [exec] 	at android.test.ActivityInstrumentationTestCase2.runTest(ActivityInstrumentationTestCase2.java:192)
     [exec] 	at android.test.AndroidTestRunner.runTest(AndroidTestRunner.java:191)
     [exec] 	at android.test.AndroidTestRunner.runTest(AndroidTestRunner.java:176)
     [exec] 	at android.test.InstrumentationTestRunner.onStart(InstrumentationTestRunner.java:555)
     [exec] 	at android.app.Instrumentation$InstrumentationThread.run(Instrumentation.java:1886)
     [exec] ...
     [exec] com.owncloud.android.lib.test_project.test.DeleteFileTest:..
     [exec] com.owncloud.android.lib.test_project.test.DownloadFileTest:...
     [exec] com.owncloud.android.lib.test_project.test.GetCapabilitiesTest:.
     [exec] com.owncloud.android.lib.test_project.test.GetShareesTest:.
     [exec] Failure in testGetRemoteShareesOperation:
     [exec] junit.framework.AssertionFailedError
     [exec] 	at com.owncloud.android.lib.test_project.test.GetShareesTest.testGetRemoteShareesOperation(GetShareesTest.java:131)
     [exec] 	at android.test.InstrumentationTestCase.runMethod(InstrumentationTestCase.java:214)
     [exec] 	at android.test.InstrumentationTestCase.runTest(InstrumentationTestCase.java:199)
     [exec] 	at android.test.ActivityInstrumentationTestCase2.runTest(ActivityInstrumentationTestCase2.java:192)
     [exec] 	at android.test.AndroidTestRunner.runTest(AndroidTestRunner.java:191)
     [exec] 	at android.test.AndroidTestRunner.runTest(AndroidTestRunner.java:176)
     [exec] 	at android.test.InstrumentationTestRunner.onStart(InstrumentationTestRunner.java:555)
     [exec] 	at android.app.Instrumentation$InstrumentationThread.run(Instrumentation.java:1886)
     [exec] 
     [exec] com.owncloud.android.lib.test_project.test.GetSharesTest:
     [exec] Failure in testGetShares:
     [exec] junit.framework.AssertionFailedError
     [exec] 	at com.owncloud.android.lib.test_project.test.GetSharesTest.testGetShares(GetSharesTest.java:79)
     [exec] 	at android.test.InstrumentationTestCase.runMethod(InstrumentationTestCase.java:214)
     [exec] 	at android.test.InstrumentationTestCase.runTest(InstrumentationTestCase.java:199)
     [exec] 	at android.test.ActivityInstrumentationTestCase2.runTest(ActivityInstrumentationTestCase2.java:192)
     [exec] 	at android.test.AndroidTestRunner.runTest(AndroidTestRunner.java:191)
     [exec] 	at android.test.AndroidTestRunner.runTest(AndroidTestRunner.java:176)
     [exec] 	at android.test.InstrumentationTestRunner.onStart(InstrumentationTestRunner.java:555)
     [exec] 	at android.app.Instrumentation$InstrumentationThread.run(Instrumentation.java:1886)
     [exec] 
     [exec] com.owncloud.android.lib.test_project.test.GetUserAvatarTest:
     [exec] Failure in testGetUserAvatar:
     [exec] junit.framework.AssertionFailedError
     [exec] 	at com.owncloud.android.lib.test_project.test.GetUserAvatarTest.testGetUserAvatar(GetUserAvatarTest.java:62)
     [exec] 	at android.test.InstrumentationTestCase.runMethod(InstrumentationTestCase.java:214)
     [exec] 	at android.test.InstrumentationTestCase.runTest(InstrumentationTestCase.java:199)
     [exec] 	at android.test.ActivityInstrumentationTestCase2.runTest(ActivityInstrumentationTestCase2.java:192)
     [exec] 	at android.test.AndroidTestRunner.runTest(AndroidTestRunner.java:191)
     [exec] 	at android.test.AndroidTestRunner.runTest(AndroidTestRunner.java:176)
     [exec] 	at android.test.InstrumentationTestRunner.onStart(InstrumentationTestRunner.java:555)
     [exec] 	at android.app.Instrumentation$InstrumentationThread.run(Instrumentation.java:1886)
     [exec] ..
     [exec] Error in testGetUserAvatarOnlyIfChangedAfterUnchanged:
     [exec] java.lang.NullPointerException: Attempt to invoke virtual method 'java.lang.Object java.util.ArrayList.get(int)' on a null object reference
     [exec] 	at com.owncloud.android.lib.test_project.test.GetUserAvatarTest.testGetUserAvatarOnlyIfChangedAfterUnchanged(GetUserAvatarTest.java:74)
     [exec] 	at android.test.InstrumentationTestCase.runMethod(InstrumentationTestCase.java:214)
     [exec] 	at android.test.InstrumentationTestCase.runTest(InstrumentationTestCase.java:199)
     [exec] 	at android.test.ActivityInstrumentationTestCase2.runTest(ActivityInstrumentationTestCase2.java:192)
     [exec] 	at android.test.AndroidTestRunner.runTest(AndroidTestRunner.java:191)
     [exec] 	at android.test.AndroidTestRunner.runTest(AndroidTestRunner.java:176)
     [exec] 	at android.test.InstrumentationTestRunner.onStart(InstrumentationTestRunner.java:555)
     [exec] 	at android.app.Instrumentation$InstrumentationThread.run(Instrumentation.java:1886)
     [exec] 
     [exec] com.owncloud.android.lib.test_project.test.GetUserQuotaTest:.
     [exec] com.owncloud.android.lib.test_project.test.MoveFileTest:.
     [exec] com.owncloud.android.lib.test_project.test.OwnCloudClientManagerFactoryTest:.....
     [exec] com.owncloud.android.lib.test_project.test.OwnCloudClientTest:...........
     [exec] com.owncloud.android.lib.test_project.test.ReadFileTest:
     [exec] Failure in testReadFile:
     [exec] junit.framework.AssertionFailedError
     [exec] 	at com.owncloud.android.lib.test_project.test.ReadFileTest.testReadFile(ReadFileTest.java:70)
     [exec] 	at android.test.InstrumentationTestCase.runMethod(InstrumentationTestCase.java:214)
     [exec] 	at android.test.InstrumentationTestCase.runTest(InstrumentationTestCase.java:199)
     [exec] 	at android.test.ActivityInstrumentationTestCase2.runTest(ActivityInstrumentationTestCase2.java:192)
     [exec] 	at android.test.AndroidTestRunner.runTest(AndroidTestRunner.java:191)
     [exec] 	at android.test.AndroidTestRunner.runTest(AndroidTestRunner.java:176)
     [exec] 	at android.test.InstrumentationTestRunner.onStart(InstrumentationTestRunner.java:555)
     [exec] 	at android.app.Instrumentation$InstrumentationThread.run(Instrumentation.java:1886)
     [exec] 
     [exec] com.owncloud.android.lib.test_project.test.ReadFolderTest:
     [exec] Failure in testReadFolder:
     [exec] junit.framework.AssertionFailedError
     [exec] 	at com.owncloud.android.lib.test_project.test.ReadFolderTest.testReadFolder(ReadFolderTest.java:84)
     [exec] 	at android.test.InstrumentationTestCase.runMethod(InstrumentationTestCase.java:214)
     [exec] 	at android.test.InstrumentationTestCase.runTest(InstrumentationTestCase.java:199)
     [exec] 	at android.test.ActivityInstrumentationTestCase2.runTest(ActivityInstrumentationTestCase2.java:192)
     [exec] 	at android.test.AndroidTestRunner.runTest(AndroidTestRunner.java:191)
     [exec] 	at android.test.AndroidTestRunner.runTest(AndroidTestRunner.java:176)
     [exec] 	at android.test.InstrumentationTestRunner.onStart(InstrumentationTestRunner.java:555)
     [exec] 	at android.app.Instrumentation$InstrumentationThread.run(Instrumentation.java:1886)
     [exec] 
     [exec] com.owncloud.android.lib.test_project.test.RemoveShareTest:.
     [exec] com.owncloud.android.lib.test_project.test.RenameFileTest:....
     [exec] com.owncloud.android.lib.test_project.test.SimpleFactoryManagerTest:....
     [exec] com.owncloud.android.lib.test_project.test.SingleSessionManagerTest:....
     [exec] com.owncloud.android.lib.test_project.test.UpdatePrivateShareTest:.
     [exec] com.owncloud.android.lib.test_project.test.UpdatePublicShareTest:.
     [exec] com.owncloud.android.lib.test_project.test.UploadFileTest:...
     [exec] Test results for InstrumentationTestRunner=....F...........F.F.F...E...................F.F.
     [exec] .................
     [exec] Time: 413.235
     [exec] 
     [exec] FAILURES!!!
     [exec] Tests run: 58,  Failures: 6,  Errors: 1
     [exec] 
     [exec] 

```

Verifica-se que, dos 58 testes existentes, 6 falham e ocorre 1 erro num deles.
Posto isto, pode-se deduzir que existem testes que exercitam componentes que podem já ter sido alteradas e desta forma encontram-se desatualizados, levando à sua falha.

Numa perspetiva geral, a maior parte dos componentes da camada da aplicação não possui (atualmente) testes implementados. Pode-se concluir que esta falta de testes é um defeito do projeto, pois dificulta a validação da maioria dos módulos.

## *Identify a new bug and/or correct a bug*
Após alguns testes à aplicação, não detetámos *bugs*, portanto decidimos resolver um dos *bugs* indicados nas *issues* do *GitHub*, escolhendo a *issue* 1562: https://github.com/owncloud/android/issues/1562.

Para resolver a *issue* 1562, analisamos a situação usando a interface do utilizador e, depois, analisamos o código. Ao nível da interface, verificámos que o problema ocorria tanto com ficheiros como com pastas. Ao nível do código, verificámos que, por vezes, o *ArrayList* retornado pelo método “getData” em “ShareActivity” nem sempre tinha apenas um elemento, tendo mais do que um, isto é, tinha uma hiperligação para *User* e uma para *Public*. Depois, analisámos o código da classe “RemoteOperationResult”, que é usada por diversas classes, e descobrimos que possuía um método chamado “setData”. De seguida, identificámos, com a ajuda da capacidade “Find Usages” do *Android Studio*, todos os locais em que “setData” era usado e verificámos que apenas as classes “ShareToRemoteOperationResultParser” e “ReadRemoteFolderOperation” poderiam criar um *ArrayList* com mais de um elemento. Após analisarmos o que essas duas classes adicionavam ao *ArrayList* delas, antes de ser aplicado o método “setData”, entendemos que o problema não estava em “ReadRemoteFolderOperation”, pois esta classe trata os dados de uma pasta remota e seus ficheiros-filhos. Portanto, verificamos se havia diferentes tipos de hiperligações e descobrimos em “ShareType” que isso era verdade, sendo muito perigoso apenas aceitar hiperligações públicas em “ShareToRemoteOperationResultParser” (sabemos agora que de certeza que isso iria trazer problemas), portanto decidimos usar a correção apresentada na *issue* com algumas modificações, que consideramos melhorias, pois achamos que aquilo que foi considerado um *workaround* temporário é a verdadeira solução ao problema, não trazendo efeitos negativos ao funcionamento da aplicação.

Mais tarde, confirmamos que, como esperado, as hiperligações eram adicionadas por ordem de criação, então, se a hiperligação pública fosse a primeira a ser criada (ou não fosse criada), nunca havia problema, caso contrário, o problema ocorreria.

Contribuições

Diogo Cruz - up201105483@fe.up.pt - 25%

Luís Barbosa - up201405729@fe.up.pt - 25%

Paulo Santos - up201403745@fe.up.pt - 25%

Sérgio Ferreira - up201403074@fe.up.pt - 25%