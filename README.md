package com.example.app

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.Image
import androidx.compose.foundation.background
import androidx.compose.foundation.clickable
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.foundation.shape.CircleShape
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.draw.clip
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.res.painterResource
import androidx.compose.ui.text.TextStyle
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import com.example.app.ui.theme.AppTheme

private val messages: List<MyMessage> = listOf(
    MyMessage("Probando JetPack Compose !", "Voy por buen camino y estoy aprendiendo mucho. Cada día descubro nuevas funcionalidades."),
    MyMessage("Probando JetPack Compose 2", "Voy por buen camino. Me siento más cómodo con la sintaxis."),
    MyMessage("Probando JetPack Compose 3", "Voy por buen camino. La estructura composable es muy flexible."),
    MyMessage("Probando JetPack Compose 4", "Voy por buen camino. Estoy entendiendo mejor la gestión de estados."),
    MyMessage("Probando JetPack Compose 5", "Voy por buen camino. Pronto podré hacer aplicaciones más avanzadas.")
)

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            AppTheme {
                Column(modifier = Modifier.fillMaxSize()) {
                    PersonalData(name = "Charles")
                    MyMessages(messages)
                }
            }
        }
    }
}

@Composable
fun PersonalData(name: String) {
    Column(
        modifier = Modifier.padding(10.dp).fillMaxWidth(),
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Image(
            painter = painterResource(R.drawable.jetpack),
            contentDescription = "Esta es una imagen de JetPack",
            modifier = Modifier.height(150.dp)
        )
        Text(text = "Mi nombre es $name", style = MaterialTheme.typography.headlineLarge)
        Text(text = "Soy Addel")
        Text(text = "Estoy aprendiendo JetPack Compose")
    }
}

data class MyMessage(val title: String, val body: String)

@Composable
fun MyMessages(messages: List<MyMessage>) {
    LazyColumn(modifier = Modifier.fillMaxSize()) {
        items(messages) { message ->
            MyComponent(message)
        }
    }
}

@Composable
fun MyComponent(message: MyMessage) {
    Row(
        modifier = Modifier
            .background(MaterialTheme.colorScheme.background)
            .padding(15.dp)
    ) {
        MyImage()
        MyTexts(message)
    }
}

@Composable
fun MyImage() {
    Image(
        painter = painterResource(R.drawable.red_bmw_m4_nu),
        contentDescription = "Este es mi carro favorito",
        modifier = Modifier
            .size(80.dp)
            .clip(CircleShape)
            .background(MaterialTheme.colorScheme.primary)
    )
}

@Composable
fun MyTexts(message: MyMessage) {

    var expanded by remember { mutableStateOf(false) }

    Column(modifier = Modifier.padding(start = 8.dp).clickable {
        expanded = !expanded
    }) {
        MyText(
            message.title,
            MaterialTheme.colorScheme.primary,
            MaterialTheme.typography.headlineMedium
        )

        Spacer(modifier = Modifier.height(16.dp))

        MyText(
            message.body,
            MaterialTheme.colorScheme.primary,
            MaterialTheme.typography.headlineSmall,
            if (expanded) Int.MAX_VALUE else 1
        )
    }
}

@Composable
fun MyText(text: String, color: Color, style: TextStyle, lines: Int = Int.MAX_VALUE) {
    Text(text, color = color, style = style, maxLines = lines)
}

@Preview(showSystemUi = true)
@Composable
fun PreviewComponents() {
    AppTheme {
        Column {
            PersonalData(name = "Charles")
            MyMessages(messages)
        }
    }
}
