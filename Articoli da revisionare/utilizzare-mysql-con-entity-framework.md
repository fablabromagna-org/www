---
title: 'Utilizzare mySQL con Entity Framework'
date: Fri, 10 Apr 2015 16:06:00 +0000
draft: false
tags: ['.NET', '.NET', 'Entity Framework', 'mySQL', 'mySQL', 'WPF', 'XAML', 'XAML']
---

Una delle tecnologie che negli ultimi anni hanno contribuito a rendere più semplice l’utilizzo di un RDBMS all’interno di una qualsiasi applicazione .NET è senza dubbio Entity Framework (EF). L’uso di Entity Framework è spiegato nel sito di riferimento [https://msdn.microsoft.com/en-us/data/ef.aspx](https://msdn.microsoft.com/en-us/data/ef.aspx "https://msdn.microsoft.com/en-us/data/ef.aspx") ma tutti gli esempi si appoggiano al db di casa Microsoft SQL Server. Trovare sul web una sessione di SQL Server economica è abbastanza difficile mentre l’hosting gratuito per mySQL è una cosa ormai normale. Anche sul sito di mySQL le cose non vanno meglio, l’esempio risale a qualche anno fa e utilizza WinForm e un vecchio Visual Studio [http://dev.mysql.com/doc/connector-net/en/connector-net-tutorials-entity-framework-winform-data-source.html](http://dev.mysql.com/doc/connector-net/en/connector-net-tutorials-entity-framework-winform-data-source.html "http://dev.mysql.com/doc/connector-net/en/connector-net-tutorials-entity-framework-winform-data-source.html") Quindi ho voluto fare una prova con la mia sessione di mySQL che ho in linea presso UnoEuro. Funziona! Ecco l’elenco delle cose che servono: - I componenti client di mySQL scaricabili dal sito di mySQL [http://dev.mysql.com/downloads/file.php?id=456518](http://dev.mysql.com/downloads/file.php?id=456518 "http://dev.mysql.com/downloads/file.php?id=456518") - Le DLL per .NET che consentono di utilizzare EF con mySQL impacchettare in questo Nuget [https://www.nuget.org/packages/MySql.Data.Entity/](https://www.nuget.org/packages/MySql.Data.Entity/ "https://www.nuget.org/packages/MySql.Data.Entity/") Si parte Scarichiamo componenti client di mySQL [![Immagine 005](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine005_thumb.png "Immagine 005")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine005.png) [![Immagine 006](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine006_thumb.png "Immagine 006")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine006.png)   Li installiamo [![Immagine 007](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine007_thumb.png "Immagine 007")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine007.png)   Una volta installati i componenti client mi accorgo che dentro all’installazione c’è un manager per mySQL molto molto bello (si chiama mySQL Workbench) Lo lancio e creo subito una connessione verso il mio DB [![Immagine 008](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine008_thumb.png "Immagine 008")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine008.png) che una volta attivata mostra un bell’ambiente di amministrazione (finalmente!!! ci voleva proprio!) [![Immagine 009](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine009_thumb.png "Immagine 009")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine009.png)   Apro il mio db ed ecco il risultato [![Immagine 010](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine010_thumb.png "Immagine 010")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine010.png)   [![Immagine 011](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine011_thumb.png "Immagine 011")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine011.png)   Ora passiamo a Visual Studio. Creiamo una applicazione WPF (la prossima prova è per ASP.NET MVC 5) e lanciamo la console di gestione di NuGet [![Immagine 013](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine013_thumb.png "Immagine 013")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine013.png)   dalla quale lanciamo l’installazione del Data Entity per mySQL [![Immagine 014](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine014_thumb.png "Immagine 014")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine014.png) [![Immagine 015](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine015_thumb.png "Immagine 015")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine015.png)   A questo punto è necessario compilare prima di proseguire, pena il messaggio di errore riportato più avanti!!! Bene! Ora, aprendo il file App.config, noto che la stringa di connessione non è presente. [![Immagine 016](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine016_thumb.png "Immagine 016")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine016.png) Dobbiamo trovare un modo per farcela creare… VS sa come fare!! Il sistema è quello di creare un nuovo Entity Data Model con il suo wizard quindi lo aggiungiamo cliccando destro sul progetto –> Aggiungi – > Nuovo Elemento [![Immagine 017](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine017_thumb.png "Immagine 017")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine017.png)   [![Immagine 018](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine018_thumb.png "Immagine 018")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine018.png)   [![Immagine 019](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine019_thumb.png "Immagine 019")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine019.png)   Ecco. Manca la stringa di connessione!! (la combobox è vuota) Possiamo quindi lanciare il wizard per crearne una nuova [![Immagine 020](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine020_thumb.png "Immagine 020")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine020.png)   Procediamo scegliendo il giusto provider (vogliamo usare mySQL, non SQL Server) [![Immagine 021](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine021_thumb.png "Immagine 021")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine021.png) [![Immagine 022](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine022_thumb.png "Immagine 022")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine022.png)   A questo punto possiamo inserire i dati di login forniti da Unoeuro… [![Immagine 023](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine023_thumb.png "Immagine 023")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine023.png)   Cavolo!  Funziona! [![Immagine 024](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine024_thumb.png "Immagine 024")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine024.png)   Per accendere il pulsante avanti, è necessario che tutto sia a posto come in questo screen shoot [![Immagine 025](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine025_thumb.png "Immagine 025")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine025.png)   Attenti… qualcosa non funziona come dicevamo. Ci sono alcuni componenti del pacchetto NuGet infatti che vanno compilati! Se non abbiamo compilato la soluzione prima di partire, appare questo errore [![Immagine 026](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine026_thumb.png "Immagine 026")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine026.png)   Se tutto è OK, appare la stringa di connessione in App.config [![Immagine 027](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine027_thumb.png "Immagine 027")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine027.png)   Il wizard ha letto la mia tabella su mySQL e l’ha “Ormizzata” a dovere!!! :-)```
namespace WpfApplication3
{
    using System;
    using System.Collections.Generic;
    using System.ComponentModel.DataAnnotations;
    using System.ComponentModel.DataAnnotations.Schema;
    using System.Data.Entity.Spatial;

    \[Table("maurizioconti\_com\_db.tblDiplomati")\]
    public partial class tblDiplomati
    {
        \[Key\]
        \[Column(Order = 0)\]
        \[StringLength(50)\]
        public string Nome { get; set; }

        \[Key\]
        \[Column(Order = 1)\]
        \[StringLength(50)\]
        public string Cognome { get; set; }

        \[Key\]
        \[Column(Order = 2)\]
        public int id { get; set; }
    }
}

```  E poi ha creato il DbContext in modo automatico.```
namespace WpfApplication3
{
    using System;
    using System.Data.Entity;
    using System.ComponentModel.DataAnnotations.Schema;
    using System.Linq;

    public partial class mySQLdb : DbContext
    {
        public mySQLdb()
            : base("name=mySQLdb")
        {
        }

        public virtual DbSet<tblDiplomati> tblDiplomati { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Entity<tblDiplomati>()
                .Property(e => e.Nome)
                .IsUnicode(false);

            modelBuilder.Entity<tblDiplomati>()
                .Property(e => e.Cognome)
                .IsUnicode(false);
        }
    }
}
```  Fatto! Ora andiamo a usare questo Data Model ad esempio scrivendo una roba del genere lato XAML```
<Window x:Class\="WpfApplication3.MainWindow"
        xmlns\="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x\="http://schemas.microsoft.com/winfx/2006/xaml"
        Title\="MainWindow" Height\="350" Width\="525"\>
    <Grid\>
        <Grid.RowDefinitions\>
            <RowDefinition Height\="50"\></RowDefinition\>
            <RowDefinition\></RowDefinition\>
        </Grid.RowDefinitions\>
        
        <StackPanel Orientation\="Horizontal"\>
            <Button Width\="100" Margin\="5" Click\="Button\_Click"\>Go</Button\>
        </StackPanel\>
        <DataGrid Name\="dgDati" Grid.Row\="1"\></DataGrid\>
    </Grid\>
</Window\>
```  e una roba del genere lato C#```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Shapes;

using System.Data.Entity;

namespace WpfApplication3
{
    /// <summary>
    /// Logica di interazione per MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private void Button\_Click(object sender, RoutedEventArgs e)
        {
            mySQLdb db = new mySQLdb();
            db.tblDiplomati.Load();

            dgDati.ItemsSource = db.tblDiplomati.Local;
        }
    }
}
```  Provare per credere   [![Immagine 30](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine30_thumb.png "Immagine 30")](http://fablabromagna.org/blog/wp-content/uploads/2015/04/Immagine30.png)