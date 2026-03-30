# Use The PAC-Admin

1. Our team are administrators of a number of Azure DevOps organizations and each organization represents one of our customers.
2. In each Azure DevOps organization there can be one ore more Azure DevOps projects and each project is for a software development project for one of our customer. 
3. There are cases where for a customer there may be more than one Azure DevOps project in their Azure DevOps organization, but the norm is that there is a project or two for each customer: one project is always a Power Platform Project and the other, when present, is a Dynamic 365 ERP Finance and Operation Project.

4. In the most frequent case in which there is a Power Platform Projecto our team has developed automations in Azure DevOps as pipelines that can extract solutions as managed solution from a Power Platform Environment designated as a development and deploy them to some target environments that in most cases are designatedd as TEST (tst) and PRODUCTION (PRD).

5. The automations in Azure DevOps currently use the PAC CLI ( Microsoft Power Platform CLI: https://learn.microsoft.com/en-us/power-platform/developer/cli/introduction?tabs=windows ), therefore they are PowerShell scripts that use the different commands available in the Microsoft Power Platform CLI to extract solutions from one environnment and deploy then to target environments TST and PRD.

7. For each customer and therefore for each Azure DevOps organization there is a Service Account that we call generically sa1234@wgs.wuerth.com for example SA3956dynamics01@wgs.wuerth.com is one of these accounts. In this naming convention the sa token stands for Service Account the four figures are the customer's code and the wgs.wuerth.com is the WGS tenant; all the customers in fact have the identities of their employes on the same Azure Tenant that is deignated as WGS. 

8. The SA (Service Acccount) account used in the Azure DevOp pipelines are set as System Administrator of each corresponding Power Platform environment, for example SA3956dynamics01@wgs.wuerth.com will be the System Administrator for the Power Platform Environments DEV3956, TST3956 and PRD3956 and the same scheme is applied to the other customers, each with theeir on SA and four figure customer code and the correspondig minimum set of three Power Platform environment DEV, TST and PRD.

9. For each Power Platform Environment the corresponding users are registered manually and assigned the corresponding roles on each environment by the system administrator of the environment.

10. The present automations scripts that use the PAC CLI autheticate to each Power Platform Environment with which they interact with the SA account, for example wi th SA3956dynamics01@wgs.wuerth.com to perform PAC CLI commands and operation over the set of power platform environments DEV3956, TST3956 and PRD3956.  

11. 


esempio pratico:
 
Automation and Self-Service Goals: Luca emphasized the need for automation and self-service provisioning, with Martin's infrastructure team responsible for environment creation, aiming to reduce manual intervention and enable business teams to provision F&O instances after infrastructure handover.


per questo punto del vostro incontro di ieri, serve esattamente l'identita' di cui stiamo parlando.

dovra' poi avere ulteriori permessi che per ora non ha, ma comunque e' necessaria, non c'e' scampo.
 
ma Yorick continua a dire che lo deve fare infra, quindi non la dobbiamo avere noi, ma heiko/martin bauer
 
Thursday 9:48 AM Meeting ended: 47m 50s 

 
Friday 8:59 AM Meeting started

 
E la prima volta pero mi e stato richesto di firmare il documento con FPSign.
 
https://wuerth-it.fp-sign.com/de_DE/auth/sso
FP Sign - FP-Sign Login
Melden Sie sich bei FP Sign an, um Dokumente sofort und von überall zu signieren!
 
se vi serve anche a voi 
 
 
quando qualcuno chiede a WIT-DE di fare cose che hanno senso
 
 
questo secondo nano banana 2 il telefono senza fili
 
anche:
 
Friday 9:37 AM Meeting ended: 38m 33s 

 
siccome parliamo sempre di brutte cose, oggi approfitto di una bella notizia e ve la comunico se non la sapevate gia':
la piattaforma SORA di OPENAI e' stata chiusa.
 
niente piu' fiumi prosciugati per fare video con IA.
 
non interessava a nessuno e anche l'unico che aveva detto che la avrebbe usata (Disney) se non ho capito male si e' tirato indietro.
 
Belacca, Francesco
siccome parliamo sempre di brutte cose, oggi approfitto di una bella notizia e ve la comunico se non la sapevate gia': la piattaforma SORA di OPENAI e' stata chiusa.   niente piu' fiumi prosciugati p…
ni....io ho letto cose un po' diverse....ma non mi sono interessato moltissimo.....tipo che dopo che Dysney ha firmato contratti miliardari, hanno chiuso Sora (poi chissà se l'hanno chiusa per lasciarla in esclusiva a Disney o se l'hanno messa nel culo a Disney).
Ma poi in realtà mi sembra che abbiano chiuso la APP non la piattaforma.....ma vabo...whatever...costava così tanto che non l'avrei cmq mai usata 
 
Everyone non posso partecipare stamattina
 
9:00 AM Meeting started

 
Io sono con la Cunaccia... arrivo asap
 
9:28 AM Meeting ended: 28m 46s 

 