import { Header } from "@/components/Header";
import { Button } from "@/components/ui/button";
import { VideoCard } from "@/components/VideoCard";
import { Play } from "lucide-react";
import { Link } from "react-router-dom";
import heroImage from "@/assets/hero-bg.jpg";

const mockVideos = [
  {
    id: "1",
    title: "Introduction to Video Streaming",
    thumbnail: "https://images.unsplash.com/photo-1492619375914-88005aa9e8fb",
    views: "10K views",
    uploadedAt: "2 days ago"
  },
  {
    id: "2",
    title: "Amazing Tech Review 2024",
    thumbnail: "https://images.unsplash.com/photo-1486312338219-ce68d2c6f44d",
    views: "15K views",
    uploadedAt: "1 week ago"
  },
  {
    id: "3",
    title: "Cooking Tutorial: Perfect Pasta",
    thumbnail: "https://images.unsplash.com/photo-1498050108023-c5249f4df085",
    views: "8K views",
    uploadedAt: "3 days ago"
  },
  {
    id: "4",
    title: "Travel Vlog: Tokyo Adventure",
    thumbnail: "https://images.unsplash.com/photo-1461749280684-dccba630e2f6",
    views: "22K views",
    uploadedAt: "5 days ago"
  },
  {
    id: "5",
    title: "Music Production Tutorial",
    thumbnail: "https://images.unsplash.com/photo-1488590528505-98d2b5aba04b",
    views: "12K views",
    uploadedAt: "1 day ago"
  },
  {
    id: "6",
    title: "Gaming Highlights Compilation",
    thumbnail: "https://images.unsplash.com/photo-1519389950473-47ba0277781c",
    views: "30K views",
    uploadedAt: "4 days ago"
  }
];

const Index = () => {
  return (
    <div className="min-h-screen bg-background">
      <Header />
      
      <section className="relative h-[600px] overflow-hidden">
        <div 
          className="absolute inset-0 bg-cover bg-center"
          style={{ backgroundImage: `url(${heroImage})` }}
        >
          <div className="absolute inset-0 bg-gradient-to-r from-background via-background/80 to-transparent" />
        </div>
        
        <div className="relative container mx-auto px-4 h-full flex items-center">
          <div className="max-w-2xl space-y-6">
            <h1 className="text-6xl font-bold leading-tight">
              Share Your Stories
              <span className="block text-primary">Watch The World</span>
            </h1>
            <p className="text-xl text-muted-foreground">
              Upload, share, and discover amazing videos from creators around the globe. Join millions of viewers and creators today.
            </p>
            <div className="flex gap-4">
              <Link to="/upload">
                <Button variant="hero" size="lg" className="gap-2">
                  <Play className="h-5 w-5" fill="currentColor" />
                  Start Creating
                </Button>
              </Link>
              <Button variant="outline" size="lg">
                Explore Videos
              </Button>
            </div>
          </div>
        </div>
      </section>

      <section className="container mx-auto px-4 py-12">
        <h2 className="text-3xl font-bold mb-8">Trending Videos</h2>
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          {mockVideos.map((video) => (
            <VideoCard key={video.id} {...video} />
          ))}
        </div>
      </section>
    </div>
  );
};

export default Index;
